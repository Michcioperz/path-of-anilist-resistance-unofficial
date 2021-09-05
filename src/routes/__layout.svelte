<svelte:head>
  <title>Path of Anilist Resistance (UNOFFICIAL)</title>
</svelte:head>

<style>
  .container {
    margin-left: auto;
    margin-right: auto;
    max-width: 1000px;
  }
  .container > * {
    margin-top: 1rem;
    margin-bottom: 1rem;
  }
</style>

<div class="container">
  <header>
    <h1>Path of Anilist Resistance (UNOFFICIAL)</h1>
  </header>

  <main>
    <slot></slot>
  </main>

  <footer>
    This app is not affiliated with Anilist.
  </footer>
</div>
