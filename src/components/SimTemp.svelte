<script>
  import { onMount } from "svelte";
  import * as d3 from 'd3';
  import { fly } from 'svelte/transition'

  export let index;

  let oringData = [];
  let shuttleImage = "shuttle.png";
  let explosionImage = "explosion.png";

  onMount(async () => {
      const res = await fetch('o-ring.csv'); 
      const csv = await res.text();
      oringData = d3.csvParse(csv, d3.autoType)

      console.log(oringData);
  });

  // Inserts Slider
  let slider_value = 36;
  let slider_label = "";
  $: slider_label = slider_value;

  // Inserts probability based on linear regression model from notebook
  // 0 = no explosion, 1 = explosion
  function probability_explosion(inputTemp){
    const randomNum = Math.random();
    const intercept = 14.861927086730798;
    const coef = -0.22950051284078968;
    let prob, result;

    prob = Math.exp(intercept + coef*inputTemp)/(1+Math.exp(intercept + coef*inputTemp));
    result = randomNum < prob ? 1 : 0;
    return result
  }

  // Animation
  let state = true;
  let prob = 0;

  function animate(){
    state = !state
    prob = probability_explosion(slider_value)
    console.log(probability_explosion(slider_value))
  }

</script>

<main>
  <section id="intro">
    <h1> What Were the Odds of the Challenger Disaster? </h1>
    <p>Use the slider to select a temperature. Press "Launch" to start the simulation. <br/> For best viewing experience, use full screen mode. </p>
  </section>

  <section id="simulation">
    <!-- Slider -->
    <div class="slider">
      <label> {slider_label}°F <br/> </label>

      <input 
        type="range" 
        min="20" 
        max="100" 
        bind:value={slider_value} 
      />  
    </div>

    <!-- Launch button -->
    <div id="launch">
      <button 
        id="launch" 
        type="launch" 
        on:click={animate}
      >
      {state ? 'Launch' : 'Restart'}
      </button>
    </div>

    <!-- Shuttle -->
    {#if state}
      <img class="shuttle" src={shuttleImage} out:fly="{{ y: -300, duration: prob == 0 ? 1200 : 0 }}"/>
    {:else}
      <img class="explosion" src={explosionImage} in:fly="{{ y: 0, duration: prob == 1 ? 1200 : 0 }}" style="opacity:{prob == 1 ? "100" : "0" }" />
    {/if}
  </section>
    
  <section id="note">
    <p> Note: The slider range is based on the temperature history of Cape Canaveral, Florida(where the space shuttle was launched) in 1986.</p>
  </section>
</main>

<style>
  input[type="range"]{
    width: 150px; 
    accent-color: red;
    position: relative;
    top:82px;
    transform: rotate(-90deg) scale(1.2);
  }

  label{
    color: white;
  }

  .slider{
    transform: translate(-250px, 10px);
  }

  button[type="launch"]{
    transform: translate(240px, -30px) scale(1.2);
    border-radius: 4px;
  }
  
  .shuttle{
    transform: translate(-50px, 200px);
    position: absolute;
  }

  .explosion{
    transform: translate(-170px, 110px);
    position: absolute;
  }

  #simulation{
    height: 80vh;
    text-align: center;
    padding: 1em;
    margin: 0 0 0 0;
    background: rgb(2,0,36);
    background: linear-gradient(180deg, rgba(2,0,36,1) 0%, rgba(9,9,121,1) 0%, rgba(44,82,128,1) 0%, rgba(0,212,255,1) 86%);
  }

  #note{
    margin: 0 0 0 0;
    text-align: left;
  }
</style>
