<script lang="ts">
  import Router from 'svelte-spa-router'
  import Game from './routes/Game.svelte';
  import Home from './routes/Home.svelte'
  import NotFound from './routes/NotFound.svelte';
  import Host from './routes/Host.svelte';
  import { AppBar, Button, Icon, Menu, ListItem, MaterialApp } from 'svelte-materialify';
    let routes = {
    "/": Home,
    "/Game:id": Game,
    "*": NotFound
    }
</script>

<main>
<MaterialApp>
<AppBar>
  <span slot="title">multimath</span>
  <div style="flex-grow:1" />
  <Button href="/">home</Button>
  <Button href="/#/Game">play</Button>
  <Button>host</Button>
</AppBar>
</MaterialApp>
<Host/>
</main>

<style>
  :root {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
      Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }

</style>
