<svelte:head>
  <title>Path of Anilist Resistance (UNOFFICIAL)</title>
</svelte:head>

<header class="bg-purple-900 text-purple-50 p-4 mb-4 font-semibold">
  <div class="container mx-auto px-4">
    <h1>Path of Anilist Resistance (UNOFFICIAL)</h1>
  </div>
</header>

<div class="container mx-auto px-4">
  <main>
    <slot></slot>
  </main>

  <footer>
    This app is not affiliated with Anilist.
  </footer>
</div>

<style>
  @tailwind base;
  @tailwind components;
  @tailwind utilities;

  :global(.bg-planning) {
    background-color: rgb(247,154,99);
  }

  :global(.bg-watching) {
    background-color: rgb(61,180,242);
  }

  :global(.bg-paused) {
    background-color: rgb(250,122,122);
  }
</style>
