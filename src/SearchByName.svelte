<script lang="ts">
  import { Input, NavLink } from "sveltestrap";

  export let page = "Search By Name";
  export let result: any = null;
  export let search = "";

  async function handleChangeName(e: Event) {
    search = (e.target as HTMLInputElement).value;

    try {
      const resp = await fetch(
        `https://api.themoviedb.org/3/search/movie?api_key=${SVELTE_APP_MOVIE_DB_API_KEY}&query=${search}`
      );

      if (resp.status !== 200) throw await resp.text();

      result = await resp.json();
    } catch (e) {
      console.error(e);
    }
  }
</script>

<div class="container">
  <h5>Search By Name</h5>

  <div>
    {search}
  </div>

  <Input placeholder="Search By Name..." on:blur={handleChangeName} />

  <div class="d-flex flex-wrap justify-content-center">
    {#each result?.results ?? [] as movie, i}
      <div class="mx-2">
        <div style="width:200px">
          <NavLink href={`#/moviedetail/${movie.id}`}>
            <div>
              <img
                style="max-width:200px;"
                src={`https://image.tmdb.org/t/p/original/${movie?.poster_path}`}
              />
            </div>
          </NavLink>
          <h6>{movie?.original_title}</h6>
        </div>
      </div>
    {/each}
  </div>
</div>
