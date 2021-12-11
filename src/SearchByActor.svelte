<script lang="ts">
  import { Input, NavLink } from "sveltestrap";
import MovieDetails from "./MovieDetails.svelte";

  export let page = "Search By Actor";
  export let result: any = null;
  export let search = "";

  async function handleChangeName(e: Event) {
    search = (e.target as HTMLInputElement).value;

    try {
      const resp = await fetch(
        `https://api.themoviedb.org/3/search/person?api_key=${SVELTE_APP_MOVIE_DB_API_KEY}&query=${search}`
      );

      if (resp.status !== 200) throw await resp.text();

      result = await resp.json();
    } catch (e) {
      console.error(e);
    }
  }
</script>

<div class="container">
  <h5>Search Actor</h5>

  <div>
    {search}
  </div>

  <Input placeholder="Search By Name..." on:blur={handleChangeName} />

  <div class="d-flex flex-wrap justify-content-center">
    {#each result?.results ?? [] as person, i}
    
    {#if person?.profile_path}
    <div class="mx-2">
        <div style="width:200px">
          <NavLink href={`#/moviedetail/${person.id}`}>
            <div>
              <img
                style="max-width:200px;"
                src={`https://image.tmdb.org/t/p/original/${person?.profile_path}`}
              />
            </div>
          </NavLink>
          <h6>{person?.name}</h6>
        </div>
      </div>
    {/if}
    {/each}
  </div>

  <!-- <pre>
      {JSON.stringify(result?.results, null, 2)}
  </pre> -->
</div>
