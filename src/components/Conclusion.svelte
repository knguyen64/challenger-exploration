<script>
    import { onMount } from "svelte";
    import * as d3 from 'd3';

    export let index;

    let astronauts = "astronauts.png"
</script>
  
<main>
    <img 
        id="img" 
        src={astronauts} 
        width="50%"
        height="50%"
    />
</main>
  
<style>
    img {
    }
</style>