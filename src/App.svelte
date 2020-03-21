<script>
  let search = "";
  let loading = false;
  const API_URL =
    "https://api.giphy.com/v1/gifs/search?api_key=Bp2xLkD77GcFIqf7S3i15409QAgONLkb&rating=R&limit=50&q=";
  let gifs = [];

  async function formSub(event) {
    loading = true;
    gifs = [];
    const url = `${API_URL}${search}`;
    const response = await fetch(url);
    const json = await response.json();
    gifs = json.data.map(gif => gif.images.fixed_height.url);
    loading = false;
  }
</script>

<style>
  .results {
    column-count: 3;
  }
  img {
    width: 100%;
    height: auto;
  }
  .loader {
    border: 16px solid #f3f3f3; /* Light grey */
    border-top: 16px solid #3498db; /* Blue */
    border-radius: 50%;
    width: 20px;
    height: 20px;
    animation: spin 2s linear infinite;
  }

  @keyframes spin {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
</style>

<form on:submit|preventDefault={formSub}>
  <label for="search">Search</label>
  <input type="text" id="search" name="search" bind:value={search} />
  <button type="submit">Go !</button>
</form>
{#if loading}
  <div class="loader" />
{/if}

<div class="results">
  {#each gifs as gif}
    <img src={gif} alt="gif" />
  {/each}
</div>
