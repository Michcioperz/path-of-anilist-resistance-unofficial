<script>
  import { goto } from '$app/navigation';
  export let username = "";
  export function goGoManiac() {
    if (username)
      goto(`/user/${username}`);
  }
</script>

<p class="text-lg mb-2">This app is designed to maximize your anime watching productivity by helping you find the show on your Anilist that you are not currently watching, but are closest to finishing watching.</p>

<p class="text-lg mb-2">This allows you to pick something new and still optimize for getting things done.</p>

<form on:submit|preventDefault={e => goGoManiac()}>
  <p>
    <label for="username">Pick your warrior (your Anilist username):</label>
    <input class="block w-full px-2 py-2 mb-2 rounded" bind:value={username} name="username" pattern=".+" type="text">
  </p>
  <p>
    <input type="submit" class="inline-block cursor-pointer p-2 bg-blue-500 hover:bg-blue-400 transition ease-in duration-150 text-white rounded" value="Isekai me!">
  </p>
</form>
