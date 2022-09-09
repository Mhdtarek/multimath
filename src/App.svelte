<script lang="ts">
  import Router from 'svelte-spa-router'
  import Game from './routes/Game.svelte';
  import Home from './routes/Home.svelte'
  import NotFound from './routes/NotFound.svelte';
  import Host from './routes/Host.svelte';
  import { AppBar, Button, Icon, Menu, ListItem, MaterialApp } from 'svelte-materialify';
    let routes = {
    "/": Home,
    "/Game/:id": Game,
    "/HostGame": Host,
    "*": NotFound
    }
</script>

<main>
<MaterialApp>
<AppBar>
  <span slot="title"><a href="/#/">multimath</a></span>
  <div style="flex-grow:1" />
  <Button><a href="/#/HostGame">Skapa Spel</a></Button>
</AppBar>
</MaterialApp>
<Router {routes}/>
</main>

<style>
  :root {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
      Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }
  a, a:hover, a:focus, a:active {
      text-decoration: none;
      color: inherit;
 }
</style>
