<script lang="ts">
import { onMount } from "svelte";


    export let params:{id?: string | null} = {};
    export let moviedetail: any = null;

    onMount(async () => {
        console.log(params?.id);

    try {
      const resp = await fetch(
        `https://api.themoviedb.org/3/movie/${params.id}?api_key=${SVELTE_APP_MOVIE_DB_API_KEY}`
      );

      if (resp.status !== 200) throw await resp.text();

      moviedetail = await resp.json();
    } catch (e) {
      console.error(e);
    }
    })


</script>

<div>
    {JSON.stringify(params)}

    <pre>
        {JSON.stringify(moviedetail, null, 2)}
    </pre>
</div>