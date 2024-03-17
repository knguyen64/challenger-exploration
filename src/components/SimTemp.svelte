<script>
  import { onMount } from "svelte";
  import * as d3 from 'd3';
  import { fly } from 'svelte/transition'

  // export let index;

  let oringData = [];
  let shuttleImage = "shuttle.png";
  let explosionImage = "explosion.png";
  let infoBool = false;

  onMount(async () => {
      const res = await fetch('o-ring.csv'); 
      const csv = await res.text();
      oringData = d3.csvParse(csv, d3.autoType)

      // console.log(oringData);
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
    return [result, prob]
  }

  // Animation
  let state = true;
  let exploded = 0;
  let prob = '__';
  let denom = '__';
  
  function animate(){
    state = !state
    exploded = probability_explosion(slider_value)[0]
    prob = ((1 - probability_explosion(slider_value)[1])*1000)
    if(prob > 1){
      prob = Math.floor(prob).toFixed(0)
      denom = '1,000'
    } else if (prob > 0.1){
      prob = Math.floor(prob*10).toFixed(0)
      denom = '10,000'
    } else if (prob > 0.01){
      prob = Math.floor(prob*100).toFixed(0)
      denom = '100,000'
    }
    // console.log(prob)
  }

  // Animation
  let shuttle;
  let shuttleX = -200;
  let translated = false;

  //Animation
  $: if(exploded == 1 && !state){
    // if explosion
    d3.select(shuttle)
      .attr("src", explosionImage)
      .transition()
      .style('opacity', "0%")
  } else if(exploded == 0 && !state){
    // if no explosion
    d3.select(shuttle)
      .style('opacity', "100%")
      .transition()
      .style('transform', "translateY( "+ shuttleX + "px)")
      .style('opacity', "0%")
    translated = true;
  }else if(state){
    // if no launch
    d3.select(shuttle)
      .attr("src", shuttleImage)
      .style('opacity', "100%")

    if(translated == true){
      d3.select(shuttle)
        .style('transform', null)
      translated = false;
    }
  }

  function getInfo(){
    if (infoBool==true){
      infoBool=false;
    } else{
      infoBool=true;
    }
  }

</script>

<main>
  <div id="simulation-wrapper">
    <div id="intro">
      <p style="margin-block-start: 0em; margin-bottom: 5px; font-size: 23px">Investigate the probability of the Challenger launching yourself.</p>
      <p style="margin-block-start: 0em; font-size: 18px"><i>Use the slider to select a temperature. Press "Launch" to start the simulation.</i></p>
    </div>
    
    <div id="interactivity">
      <div id="interaction-box">
        <p style="display:inline"> Interaction </p> <button id="info" on:click={getInfo}> 
          <div style="    display: flex; 
          align-items: center;
          justify-content: center;
          transform: translatey(-2px);
          font-size: 15px;"> 
            <b>i</b> 
          </div></button>
      </div> 

      <div class="slider">
        <label> {slider_label}°F <br/> </label>
  
        <input 
          type="range" 
          min="20" 
          max="100" 
          bind:value={slider_value} 
          orient="vertical"
        />  
      </div>
  
      <div id="launch">
        <button 
          id="launch" 
          type="launch" 
          on:click={animate}
        >
        {state ? 'Launch' : 'Restart'}
        </button>
      </div>
      <br/>

      {#if infoBool}
        <div id="note" style="color: white; width: 90%">
          <p> Slider Note: <br/> The slider range is based on the temperature history of Cape Canaveral, Florida(where the space shuttle was launched) in 1986.</p>
          <p> Probability Note: <br/> Probabilities are based on a logistic regression model.</p>
        </div>    
      {/if}
    </div>

    <div id="simulation">
      <img class="shuttle" bind:this={shuttle} src={shuttleImage}/>
  
      <div id="probability">
        <p style="
        margin-block-start: 0.3em;
        margin-block-end: 0.3em; 
        margin-inline-start: 0.5em; 
        margin-inline-end: 0.5em;"> Your chance of success is about {prob} in {denom}</p>
      </div>  
    </div>
  </div>
</main>

<style>
  #info{
    height: 20px;
    line-height: 20px;  
    width: 20px;
    border-radius: 50%;
    text-align: center;
    background-color: white;
  }

  #info:hover{
    fill: gray;
    opacity: 0.7;
  }

  input[type="range"][orient=vertical]{
    width: 150px; 
    accent-color: red;
    position: relative;
    text-align: right;
    appearance: slider-vertical;
    }

  label{
    color: white;
    font-family: "Open Sans", sans-serif;
  }

  #interactivity{
    width:15%;
    height:auto;
    margin-top: 15px;
    margin-left: 15px;
    text-align: center;
    background-color: rgba(0, 0, 0, 0.3);
    position: absolute;
    border-radius: 10px;
    padding-top: 10px;
    padding-bottom: 10px;

  }

  button[type="launch"]{
    font-family: "Open Sans", sans-serif;
    transform: scale(1.2);
    margin-top: 10px;
    border-radius: 4px;
  }

  button:hover{
    fill: gray;
    opacity: 0.7;
  }
  
  .shuttle{
    transform: translate(0px, 80px);
    position: absolute;
    transition: opacity .25s ease-in-out;
  }

  .explosion{
    transform: translate(0px, 80px);
    position: absolute;
  }

  #simulation{
    height: 85vh;
    width: 100%;
    text-align: center;
    padding: 0em;
    margin: 0 0 0 0;
    background: rgb(11,61,145);
    background: linear-gradient(180deg, rgba(11,61,145,1) 0%, rgba(123,180,250,1) 100%);
    display: flex; 
    align-items: center;
    justify-content: center;
  }

  #note{
    font-family: "Open Sans", sans-serif;
    padding-left: 10px;
    text-align: left;
  }

  #intro{
    font-family: "Open Sans", sans-serif;
    font-size: 20px;
    margin: 0 0 0 0;
  }

  #interaction-box{
    font-family: "Open Sans", sans-serif;
    color: white;
    background-color: rgba(0, 0, 0, 0.4);
    padding-top: 10px;
    padding-bottom: 10px;
  }

  #probability{
    font-family: "Open Sans", sans-serif;
    font-size: 18px;
    transform:translate(0px, 270px);
    background-color: white;
    opacity: 0.9;
    border-radius: 8px;
  }

  #simulation-wrapper{
    padding-top: 0px;
    animation: fadein 1000ms ease-in;
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
