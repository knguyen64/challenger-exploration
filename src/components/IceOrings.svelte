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
    let width = 564;
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

    //Change page
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

    $: d3.selectAll("shuttleShape")
        .style('opacity', "100%")
        .transition()
        .style('opacity', "0%")

</script>
  
<main>
    <div style="">
        {#if page == 0}
            <p>
                <i>Effects of cold temperature on the day of the shuttle’s launch.</i>
            </p>
        {/if}
        {#if page == 1}
            <p>
                <i>Ice around the Challenger’s rocket boosters and the launch pad.</i>
            </p>
        {/if}
        {#if page == 2}
            <p>
                <i>Frozen launch pad controls the morning of the disaster.</i>
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
    <div id="interactivity" style="">
        <button 
        id="last" 
        type="last" 
        on:click={changePageBackward}
        />
        <p style="display:inline; padding-left: 5%; padding-right: 5%;  font-size:18px"> Photo {page+1} </p>
        
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
    svg{
        margin-bottom: 15px;
        margin-top: 5px;
    }
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
        fill: dark gray;
        opacity: 100%;
    }

    #last{
        border-top: 10px solid transparent;
        border-bottom: 10px solid transparent;
        border-right: 10px solid;
        background: transparent;
        border-left: none;
        cursor: pointer;
        outline: none;
        transform: translateY(-2px);
    }

    #last:hover{
        scale: 1.2;
    }

    #next{
        border-top: 10px solid transparent;
        border-bottom: 10px solid transparent;
        background: transparent;
        border-left: 10px solid;
        cursor: pointer;
        outline: none;
        border-right: none;
        transform: translateY(-2px);
    }

    #next:hover{
        scale: 1.2;
    }


    #interactivity{
        animation: fadein 700ms ease-in-out both;
    }


    rect:hover{
        fill: light gray;
        opacity: 5%;
    }

    p{
        font-size:18px
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