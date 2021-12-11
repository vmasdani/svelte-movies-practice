<script lang="ts">
  import { onMount } from "svelte";

  import { NavLink } from "sveltestrap";

  export let page = "Search By Category";
  export let result: any = null;
  export let search = "";
  export let movieList: any = null;

  async function handleFetchMovies(e: Event) {
    search = (e.target as HTMLInputElement).value;
    console.log(search);

    const resp = await fetch(
      `https://api.themoviedb.org/3/discover/movie?api_key=${SVELTE_APP_MOVIE_DB_API_KEY}&with_genres=${search}`
    );

    movieList = await resp.json();
  }

  onMount(async () => {
    try {
      const resp = await fetch(
        "https://api.themoviedb.org/3/genre/movie/list?api_key=${SVELTE_APP_MOVIE_DB_API_KEY}&language=en-US"
      );

      if (resp.status !== 200) throw await resp.text();

      result = await resp.json();
    } catch (e) {
      console.error(e);
    }
  });
</script>

<div class="container">
  <h5>Search By Category</h5>

  <div>
    {search}
  </div>

  <select placeholder="Search By Category..." on:input={handleFetchMovies}>
    {#each result?.genres ?? [] as genre}
      <option value={genre?.id}>{genre?.name}</option>
    {/each}
  </select>>

  <div class="d-flex flex-wrap justify-content-center">
    {#each movieList?.results ?? [] as movie, i}
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
