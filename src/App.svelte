<script lang="ts">
  import Router from 'svelte-spa-router'
  import Game from './routes/Game.svelte';
  import Home from './routes/Home.svelte'
  import NotFound from './routes/NotFound.svelte';
  let routes = {
    "/": Home,
    "/Game:id": Game,
    "*": NotFound
  }
  console.log(import.meta.env.VITE_FIREBASE_CONFIG)
</script>

<main>
<Router {routes}/>

</main>

<style>
  :root {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
      Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }

</style>
