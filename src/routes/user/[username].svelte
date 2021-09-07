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
  import ListEntry from '../../components/ListEntry.svelte';

  export let username, entries;
</script>

<svelte:head>
  <title>{username} – Path of Anilist Resistance</title>
</svelte:head>

<div class="md:flex mb-4">
  <h2 class="text-4xl font-semibold text-purple-900 italic self-center sm:mb-2 md:mb-0">{username}'s path</h2>
  <a class="ml-auto self-center inline-block p-2 bg-blue-500 hover:bg-blue-400 transition ease-in duration-150 text-white rounded" href="/">Pick someone else</a>
</div>

<ol class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
  {#each entries as entry}
    <ListEntry {entry}/>
  {/each}
</ol>
