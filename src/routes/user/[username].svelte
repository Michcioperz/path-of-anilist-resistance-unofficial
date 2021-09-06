<script context="module">
  export async function load({ page, fetch }) {
    const query = `
      query ($userName: String) {
        MediaListCollection(userName: $userName, status_not_in: [CURRENT, DROPPED, REPEATING], type: ANIME) {
          hasNextChunk
          user {
            name
          }
          lists {
            entries {
              status
              progress
              media {
                id
                status
                format
                title {
                  romaji
                  english
                  native
                }
                coverImage {
                  medium
                }
                season
                seasonYear
                seasonInt
                duration
                episodes
              }
            }
          }
        }
      }
    `;
    const variables = { userName: page.params.username };
    const resp = await fetch(`https://graphql.anilist.co/`, {method: 'POST', headers: {'Content-Type': 'application/json', 'Accept': 'application/json'}, body: JSON.stringify({ query: query, variables: variables })});
    const data = (await resp.json()).data.MediaListCollection;
    if (data.hasNextChunk) {
      throw "pagination was not expected, but here we are, lol";
    }
    const entrieses = data.lists.map(x => x.entries);
    let entries = entrieses.flat().filter(entry => entry.media.status != 'NOT_YET_RELEASED');
    for (const entry of entries) {
      entry.episodesLeft = (entry.media.episodes || Infinity) - entry.progress;
      entry.timeLeft = entry.media.duration * entry.episodesLeft;
    }
    entries = entries.filter(entry => entry.timeLeft > 0);
    entries.sort((lhs, rhs) => ((lhs.timeLeft - rhs.timeLeft) ||(lhs.media.seasonInt - rhs.media.seasonInt)));

    return { props: {
      username: data.user.name,
      entries,
    } };
  }
</script>

<script>
  export let username, entries;
</script>

<svelte:head>
  <title>{username} – Path of Anilist Resistance</title>
</svelte:head>

<h2>for brave weeb <u>{username}</h2>
<a href="/">Pick someone else</a>

<ol class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
  {#each entries as entry}
  <li class="bg-purple-50 flex overflow-hidden rounded shadow">
    <img src={entry.media.coverImage.medium} alt={entry.media.title.english}/>
    <div class="flex flex-col w-full py-1 px-2 ml-2">
      <h3 class="font-semibold mt-1">
        <span class="inline-block w-3 h-3 self-center rounded-full bg-{entry.status.toLowerCase()}" title={entry.status.toLowerCase()}></span>
        <a href={`https://anilist.co/anime/${entry.media.id}/`}>
            {#if 'english' in entry.media.title}
            <abbr title={entry.media.title.english}>{entry.media.title.romaji || entry.media.title.native}</abbr>
            {:else}
            {entry.media.title.romaji || entry.media.title.native}
            {/if}
        </a>
      </h3>
      <p class="text-sm text-gray-600 mb-2">
        <span>{entry.media.format}</span>
        /
        <span>{entry.media.season?.toUpperCase() || ""} {entry.media.seasonYear || "forgotten by time"} ({entry.media.status})</span> 
      </p>
      <p class="italic text-gray-800 mt-auto mb-2">
        <math>{entry.timeLeft}</math> minutes in <math>{entry.episodesLeft}</math> episodes
      </p>
    </div>
  </li>
  {/each}
</ol>
