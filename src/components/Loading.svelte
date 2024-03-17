<script>
    import { onMount } from "svelte";
    import * as d3 from 'd3';

    export let index;
    let oringData = [];

    onMount(async () => {
        const res = await fetch('o-ring.csv'); 
        const csv = await res.text();
        oringData = d3.csvParse(csv, d3.autoType)

        ps = d3.selectAll("#scroll");
        animatePs();
    });

    // Animation
    let ps = [];
    let scroll;

    function animatePs() {
        ps.transition()
        .style("margin-top", "0px")
        .transition()
        .delay(500)
        .style("margin-top", "400px")
        .on("end", animatePs);
}
</script>
  
<main>
    {#if index == 0}
    <div bind:this={scroll}>
        <p id="scroll" style="color:white; right: 45%; left: 45%; padding-top: 8%; position: absolute;"> 
            Scroll Down 
            <br/>
            <i class="arrowDown" style="color: white;"></i>
        <p/>
    </div> 
    {/if}
</main>
  
<style>
.arrowDown {
    border: solid white;
    border-width: 0 3px 3px 0;
    display: inline-block;
    padding: 15px;
    transform: rotate(45deg);
    -webkit-transform: rotate(45deg);
}

p {
    transition: margin 500ms ease-in-out;
  }

</style>