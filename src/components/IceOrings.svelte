<script>
    import { onMount } from "svelte";
    import * as d3 from 'd3';
    import { fade, fly } from 'svelte/transition'

    export let index;


    let oringData = [];
    let shuttleShape = "shuttle_shape.png";
    let oring = "Group_3-removebg-preview 1.png";
    let iceLaunch1 = "ice_launch.png";
    let iceLaunch2 = "ice_launch_2.png";
    let iceControl = "control_panel.png";
    let page = 0;
    let firstPage = 0;
    let lastPage = 2;

    onMount(async () => {
        const res = await fetch('o-ring.csv'); 
        const csv = await res.text();
        oringData = d3.csvParse(csv, d3.autoType)
    });

    //Zoom
    let width = 1000;
    let height = 800;
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

    function changePageForward(){
        if(page != lastPage){
            page++;
        } else{
            page = firstPage;
        }
    }

    function changePageBackward(){
        if(page != firstPage){
            page--;
        } else{
            page = lastPage;
        }
    }

//   $: console.log(mousePosition)

</script>
  
<main>
    <div style="transform:translate(0, 85px)">
        {#if page == 0}
            <p>
                photo 1
            </p>
        {/if}
        {#if page == 1}
            <p>
                photo 2
            </p>
        {/if}
        {#if page == 2}
            <p>
                photo 3
            </p>
        {/if}
        <svg
    bind:this={svg}
    {width}
    {height}
    viewBox="0 0 {width} {height}"
    style="max-width: 100%; height: auto;"
    xmlns="http://www.w3.org/2000/svg"
    >
    <i style="font-size: 20px">Zoom and pan across the image</i>
    <g bind:this={g}>
        {#if page == 0}

        <image 
        class="shuttleShape" 
        href={iceLaunch1} 
        {width}
        {height}        
        style=""
        />
        {/if}

        {#if page == 1}
        <image 
        class="shuttleShape" 
        href={iceLaunch2} 
        {width}
        {height}        
        style=""
        />
        {/if}

        {#if page == 2}
        <image 
        class="shuttleShape" 
        href={iceControl} 
        {width}
        {height}        
        style=""
        />
        {/if}
    </g>
    <rect {width} {height}/>
    </svg>
    <br/>
    <div style="transform:translate(0, -85px)">
        <button 
        id="last" 
        type="last" 
        on:click={changePageBackward}
        />
        <p style="display:inline;"> Page {page+1} </p>
        
        <button 
        id="next" 
        class="a"
        type="next" 
        on:click={changePageForward}
        />    
    </div>
    </div>
    

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

    #last{
        border-top: 100px solid transparent;
        border-bottom: 100px solid transparent;
        border-right: 100px solid;
        background: transparent;
        border-left: none;
        cursor: pointer;
        outline: none;
        scale: 0.1;
    }

    #last:hover{
        scale: 0.12;
    }

    #next{
        border-top: 100px solid transparent;
        border-bottom: 100px solid transparent;
        background: transparent;
        border-left: 100px solid;
        cursor: pointer;
        outline: none;
        scale: 0.1;
        border-right: none;
    }

    #next:hover{
        scale: 0.12;
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