<script>
    import { onMount } from "svelte";
    import * as d3 from 'd3';
    import { fade, fly } from 'svelte/transition'

    export let index;


    let oringData = [];
    let shuttleShape = "shuttle_with_gray_booster.png";
    let boosters = "boosters.png";
    let boostersOring = "boosters_with_oring.png";

    // let oring = "Group_3-removebg-preview 1.png";

    onMount(async () => {
        const res = await fetch('o-ring.csv'); 
        const csv = await res.text();
        oringData = d3.csvParse(csv, d3.autoType)
    });

    //Zoom
    const width = 1204;
    const height = 1000;
    let svg;
    let g;

    //Animation
    let structure = 0;
    let last_structure = 2;
    let first_structure = 0;

    let shuttleIdx;
    let boosterIdx;
    let boosterOringIdx;
    
    $: structures = {
        0:[boosterOringIdx, shuttleIdx],
        1:[shuttleIdx, boosterIdx],
        2:[boosterIdx, boosterOringIdx],
    }
    // $: console.log(structures[0][0]);


    function changePageForward(){
        if(structure != last_structure){
            structure++;
            updateStructure(structures[structure][0], structures[structure][1]);
            d3.select("#last").style("opacity", "100%")
        } 
        
        if(structure == last_structure) {
            d3.select("#next").style("opacity", "20%")
        }

    }

    function changePageBackward(){
        if(structure != first_structure){
            updateStructure(structures[structure][1], structures[structure][0]);
            structure--;
            d3.select("#next").style("opacity", "100%")
        } 
        if(structure == first_structure) {
            d3.select("#last").style("opacity", "20%")
        }


    }

    function updateStructure(oldImage, curImage){
        d3.select(oldImage)
            .transition()
            .style('opacity', 1)
            .duration(1000)
            .style("opacity", 0)

        d3.select(curImage)
            .transition()
            .style('opacity', 0)
            .duration(2000)
            .style("opacity", 1)
    }

    // $: d3.select(svg).call(zoom);
        
    // var zoom = d3.zoom()
    //     .translateExtent([[0, 0], [width, height]])
    //     .scaleExtent([1, 4])
    //     .extent([[0, 0], [width, height]])
    //     .on("zoom", zoomed);

    // function zoomed(event) {
    //     d3.select(g).attr("transform", event.transform)
    // }


</script>
  
<main>
    <svg
    bind:this={svg}
    {width}
    {height}
    viewBox="0 0 {width} {height}"
    style="max-width: 100%; height: auto; border: 2px black; outline-style: solid;
    outline-color: transparent;"
    xmlns="http://www.w3.org/2000/svg"
    >
    <g bind:this={g}>
        <image 
            bind:this={shuttleIdx}
            class="shuttleShape" 
            href={shuttleShape} 
            {width}
            {height}        
        />

        <image 
            bind:this={boosterIdx}
            class="shuttleShape" 
            href={boosters} 
            {width}
            {height}        
            style="opacity: 0"
        />

    <image 
        bind:this={boosterOringIdx}
        class="shuttleShape" 
        href={boostersOring} 
        {width}
        {height}        
        style="opacity: 0"
    />

    </g>
    </svg>
    <div id="interactivity" style="transform:translatey(0px)">
        <button 
        id="last" 
        type="last" 
        on:click={changePageBackward}
        />
        <p style="display:inline;padding-left: 3%; padding-right: 3%;"> Page {structure + 1}</p>
        
        <button 
        id="next" 
        type="next" 
        on:click={changePageForward}
        />    
    </div>

    <!-- <i style="font-size: 20px">Zoom and pan across the image</i> -->
</main>
  
<style>
    svg{
        outline-color: black;
        margin-bottom: 15px;
        margin-top: 15px;
    }
    .shuttleShape{
        position: fixed;
    }

    #interactivity{
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

    #last{
        border-top: 10px solid transparent;
        border-bottom: 10px solid transparent;
        border-right: 10px solid;
        background: transparent;
        border-left: none;
        cursor: pointer;
        outline: none;
        /* scale: 0.1; */
        opacity: 20%;
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
        /* scale: 0.1; */
        border-right: none;
        transform: translateY(-2px);
    }

    #next:hover{
        scale: 1.2;
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