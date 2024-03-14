<script>
    import { onMount } from "svelte";
    import * as d3 from 'd3';
    import { fade, fly } from 'svelte/transition'

    export let index;

    let oringData = [];

    onMount(async () => {
        const res = await fetch('o-ring.csv'); 
        const csv = await res.text();
        oringData = d3.csvParse(csv, d3.autoType)
    });
</script>
  
<main>
    {#if index == 1}
    <div id="video-container" out:fade>
        <video
        class="absolute -z-10 h-full w-full object-cover blur-sm"
        width="100%"
        muted
        autoplay
        loop
        playsinline
        disablepictureinpicture
        style="opacity:70%"
        >
            <source src="challenger-launch-clip.mp4" type="video/mp4" />
            {console.log("hi")}
        </video>

    </div>
    {/if}
</main>
  
<style>
    #video-container{
        width: 100%;
        box-sizing: border-box;
        position: absolute;
        animation: fadein 1000ms ease-in-out both;
    }

    #text-container{
        display: block;
        align-items: top;
        justify-content: top;
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