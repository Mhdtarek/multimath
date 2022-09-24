<script lang="ts">
  export let params = {};
  import MultimathGame from "./games/multiclassic/MultimathGame.svelte";
</script>

<main>
  {#if params.gameid == 1}
    <MultimathGame {params} />
  {/if}
</main>

<style>
</style>
