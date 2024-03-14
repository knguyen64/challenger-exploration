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

</script>
  
<main>
    {#if index >= 7}
        <div id='left'>
            {#each oringData as data, i}
                {#if i < 12}
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
                {/if}
            {/each}
        </div>

        <div id='right'>
            {#each oringData as data, i}
                {#if i >= 12}
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
                {/if}
            {/each}
        </div>
    {/if}
</main>
  
<style>
    .shuttleShape{
        scale: 0.5;   
    }
    .shuttleShape:hover{
        scale: 0.6;   
    }


    #left{
        position: absolute;
        left:0;
        max-width: 25%;
        animation: rightSlide 900ms ease-in;
    }

    #right{
        position: absolute;
        right: 0;
        max-width: 25%;
        animation: leftSlide 900ms ease-in;
    }

    #tooltip {
        font-family: "Arial", arial;
        font-weight: 100;
        position: fixed;
        background-color: white;
        padding: 3px;
        border: solid 1px;
    }

    @keyframes leftSlide {
    0%   { 
    transform: translatex(200%); 		
    }
    100% { 
    transform: translatex(0%); 
    }
    }

    @keyframes rightSlide {
    0%   { 
    transform: translatex(-200%); 		
    }
    100% { 
    transform: translatex(-0%); 
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
