<script>
    import { onMount } from "svelte";
    import * as d3 from 'd3';
    import { fade, fly } from 'svelte/transition'

    export let index;


    let oringData = [];
    let shuttleShape = "shuttle_shape.png";
    let oring = "o-ring.png";

    onMount(async () => {
        const res = await fetch('o-ring.csv'); 
        const csv = await res.text();
        oringData = d3.csvParse(csv, d3.autoType)
    });

    //tooltip
    const width = 1204;
    const height = 798;
    let svg;

    let g;

    $: d3.select(svg).call(zoom);
        
    var zoom = d3.zoom()
        .translateExtent([[0, 0], [width, height]])
        .scaleExtent([1, 4])
        .extent([[0, 0], [width, height]])
        .on("zoom", zoomed);

    function zoomed(event) {
        d3.select(g).attr("transform", event.transform)
    }

//   $: console.log(mousePosition)

</script>
  
<main>
    <!-- {#if index >= 5 && index < 6} -->
    <!-- <div id="shuttle-container" >
        <div id="slide-shuttle" style="{index == 6 ? "scale: 1": "scale:1"}">
            <img 
            class="shuttleShape" 
            src={oring} 
            out:fade
             />
        </div>
    </div> -->

    <svg
    bind:this={svg}
    {width}
    {height}
    viewBox="0 0 {width} {height}"
    style="max-width: 100%; height: auto; background-color:red"
    xmlns="http://www.w3.org/2000/svg"
    >
    <g bind:this={g}>
        <image 
        class="shuttleShape" 
        href={oring} 
        {width}
        {height}        
        style=""
        out:fade
    />
    </g>
    <rect {width} {height}/>
    </svg>
    <!-- {/if} -->
</main>
  
<style>
    .shuttleShape{
        position: fixed;
    }

    #shuttle-container{
        height: 50vh;
        width: 100%;
        right:0;
        position: fixed;
        display: flex;
        align-items: center;
        justify-content: center;
        /* background-color: red; */
        animation: fadein 700ms ease-in-out both;
    }

    #text-container{
        display: block;
        align-items: top;
        justify-content: top;
    }

    rect{
        opacity: 0%;
    }

    rect:hover{
        fill: light gray;
        opacity: 5%;
    }

    @keyframes fadein {
    0%   { 
    opacity: 0%; 		
    }
    100% { 
    opacity: 100%; 
    }
    }

</style>