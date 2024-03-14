<script>
    import { onMount } from "svelte";
    import * as d3 from 'd3';
    import { fly } from 'svelte/transition'

    export let index;

    let oringData = [];
    let shuttleShape = "shuttle_shape.png";
    let shuttleShapeFail = "shuttle_shape_fail.png";

    onMount(async () => {
        const res = await fetch('o-ring.csv'); 
        const csv = await res.text();
        oringData = d3.csvParse(csv, d3.autoType)
    });

    // Add tooltip
    export let selected_datapoint = false;
    let mouse_x, mouse_y;
    const setMousePosition = function(event) {
        mouse_x = event.clientX;
        mouse_y = event.clientY;
        console.log(mouse_x, mouse_y);
    }

    //animation
    $: oringDataSorted = oringData.sort(function(a, b){return  a.launch_temp - b.launch_temp});
    $: oringDataSortedSet = new Set(oringDataSorted.map(x => x.launch_temp))
    $: console.log(oringDataSortedSet)
</script>
  
<main>
    {#if index >= 8}
    <div id='shuttle-container'>
        {#each oringDataSortedSet as temp, i}
            <div id='shuttles-{i}' style="display: flex; flex-direction:column-reverse; font-size: 40px">
                <div style="font-family: Arial, arial;">
                    {temp}°F
                </div>
                {#each oringDataSorted.filter(x => x.launch_temp===temp) as data, i}
                    {#if data.o_rings_thermal_stress == 0}
                        <img 
                            class="shuttleShape" 
                            src={shuttleShape} 
                            on:mouseover={function(event) {selected_datapoint = data; setMousePosition(event)}}
                            on:mouseout={function() {selected_datapoint = undefined}}
                        />
                    {:else}
                        <img 
                        class="shuttleShape" 
                        src={shuttleShapeFail} 
                        on:mouseover={function(event) {selected_datapoint = data; setMousePosition(event)}}
                        on:mouseout={function() {selected_datapoint = undefined}}
                        />
                    {/if}
                {/each}
            </div>
        {/each}
    </div>
    {/if}
</main>
  
<style>
    .shuttleShape{
        /* scale: 0.5;   */
        padding-left: 40px;
        padding-right: 40px;
        padding-bottom: 30px;
    }
    .shuttleShape:hover{
        scale: 1.2;   
    }

    #shuttle-container{
        scale: 0.5;
        /* translate: 0px 300px; */
        width: 100%;
        position: absolute;
        animation: fadein 1000ms ease-in;
        display: flex; 
        flex-direction: row;
        justify-content: center;
        /* background-color: rgba(0, 0, 0, 0.2); */
    }

    #tooltip {
        font-family: "Arial", arial;
        font-weight: 100;
        position: fixed;
        background-color: white;
        padding: 3px;
        border: solid 1px;
    }

    #shuttles-0{
        background-color: rgba(0, 0, 0, 0.12);
    } 
    #shuttles-1{
        background-color: rgba(0, 0, 0, 0.12);
    }
    #shuttles-2{
        background-color: rgba(0, 0, 0, 0.12);
    }
    #shuttles-3{
        background-color: rgba(0, 0, 0, 0.12);
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

{#if selected_datapoint != undefined}
    <div id="tooltip" style="left: {mouse_x + 5}px; top: {mouse_y - 5}px; text-align: left; padding: 0.5em; border-color: grey; line-height:25px; border-radius: 10px;">
        <b> {selected_datapoint.o_rings_thermal_stress == 0 ? 'Successful Launch' : 'Failed Launch'} </b> <br/>
        Launch Temperature: {selected_datapoint.launch_temp}°F <br/>
        O-rings with Stress: {selected_datapoint.o_rings_thermal_stress} <br/>
        Leak Check Pressure: {selected_datapoint.leak_check_pressure} psi
    </div>
{/if}
