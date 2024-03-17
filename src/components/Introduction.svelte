<script>
    import * as d3 from 'd3';
    import { fade } from 'svelte/transition'

    export let index;

    let particles = "1348243-9bJ5LfzwGP-high_0004.avif";
    let astronauts = "astronauts.png"
</script>
  
<main>
    {#if index == 0}
    <div id="image" out:fade>
        <img 
        src={particles}
        width="100%"
        />
    </div>
    {/if}
    
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
        animation: fadein 800ms ease-in-out both;
    }

    #image{
        width: 100%;
        box-sizing: border-box;
        position: absolute;
        animation: fadein 800ms ease-in-out both;
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