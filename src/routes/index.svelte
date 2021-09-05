<script>
  import { goto } from '$app/navigation';
  export let username = "";
  export function goGoManiac() {
    if (username)
      goto(`/user/${username}`);
  }
</script>

<p>This app is designed to maximize your anime watching productivity by helping you find the show on your Anilist that you are closest to finishing watching.</p>

<form on:submit|preventDefault={e => goGoManiac()}>
  <p>
    <label for="username">Pick your warrior (your Anilist username):</label>
    <input bind:value={username} name="username" pattern=".+" type="text">
  </p>
  <p><input type="submit" value="Isekai me!"></p>
</form>
