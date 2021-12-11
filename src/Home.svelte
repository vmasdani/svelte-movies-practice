<script lang="ts">
  import { onMount } from "svelte";

  import { Input, NavLink } from "sveltestrap";

  export let name = "";
  export let movies: {
    page?: number | null;
    results?:
      | { original_title?: string | null; poster_path?: string | null ;id?: number | null }[]
      | null;
  };

  onMount(async () => {
    try {
      const resp = await fetch(
        `https://api.themoviedb.org/3/movie/popular?api_key=${SVELTE_APP_MOVIE_DB_API_KEY}`
      );

      if (resp.status !== 200) throw await resp.text();

      movies = await resp.json();
    } catch (e) {
      console.error(e);
    }
  });
</script>

<div class="container">
  <div class="my-3">
    <h5>Movie DB Svelte</h5>
  </div>
  <div class="my-2"><hr /></div>
  <div class="fw-bold">Trending movies now</div>

  <div class="d-flex flex-wrap justify-content-center">
    {#each movies?.results ?? [] as movie, i}
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
