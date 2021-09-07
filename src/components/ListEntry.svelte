<script>
  export let entry;
</script>

<li class="bg-purple-50 hover:bg-white transition ease-in duration-150 flex overflow-hidden rounded shadow">
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