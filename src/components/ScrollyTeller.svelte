<script>
    import { onMount } from "svelte";
    import Scroller from "@sveltejs/svelte-scroller";
    import SimTemp from "./SimTemp.svelte";
    import Trials1 from "./Trials1.svelte";
    import Trials2 from "./Trials2.svelte";
    import Orings from "./Orings.svelte";
    import IceOrings from "./IceOrings.svelte";
    import Introduction from "./Introduction.svelte";    
    import * as d3 from 'd3';

    let count, index, offset, progress;
    let width, height;
    let mode = false;


    function toggle(){
      window.document.body.classList.toggle('light-mode')
    }

    $: if(index == 4 && mode == false){
        // Change to light mode after introduction
        toggle();
        mode = true;
      } else if(index == 13 && mode == true){
        // Change to dark mode before takeaways
        toggle();
        mode = false;
      } else if(index > 4 && index < 13 && mode == false){
        // Change to light mode if return to section before takeaways
        toggle();
        mode = true;
      } else if(index < 4  && mode == true){
        // Change to dark mode if return to introduction
        toggle();
        mode = false;
      // } else if(index > 4  && index < 13 && mode == false){
      //   // Change to light mode if after introduction and bug happens
      //   toggle();
      //   mode = true;
      }
    

</script>

<main>
  <Scroller
  top={0.0}
  bottom={1}
  threshold={0.5}
  bind:count
  bind:index
  bind:offset
  bind:progress
>
  <div 
  class="background" 
  slot="background"
  bind:clientWidth={width}
  bind:clientHeight={height}
  >
  <Introduction {index}/>  

    <!-- <div class="progress-bars">
      <p>Current Section: <strong>{index + 1}/{count}</strong></p>
      <progress value={count ? (index + 1) / count : 0} />

      <p>Offset in current section</p>
      <progress value={offset || 0} />

      <p>Total Progress</p>
      <progress value={progress || 0} />
    </div> -->
     
  </div>

  <div class="foreground" slot="foreground">

    <section id="Title" style="min-height: 100vh;">
      <div style="color:white">
        <h1 style=""> WHAT WERE THE ODDS OF THE CHALLENGER DISASTER? </h1>
        By: Cristina De La Torre and Kathleen Nguyen
      </div>
    </section>

    <section id="Information" style="min-height: 120vh;">    
      {#if index >= 1}
        <div id=text-container-side>
          <div style="    
          background-color: white;
          opacity: 0.7;
          margin-top: 0px">
          <p style="margin-top: 20px; margin-bottom: 20px; padding-left: 20px; padding-right: 20px"> 
            On January 28, 1986, <br/> the space shuttle Challenger launched.
           </p>
        </div>
        </div>
      {/if}
    </section>

    <section id="Information">
      {#if index >= 2}
      <div id=text-container style="color: #FEFFF0"> 
        <p> 73 seconds after lift off, <br/> it exploded. </p>
      </div>
      {/if}
    </section>

    <section id="Information" style="{index > 2 ? "background-color: black" : "background-color:"}; color: #FEFFF0">
      {#if index >= 3}
      <div id=text-container>
        <p> Nasa executives believed that the space shuttle was safe to launch. So what happened? </p>
      </div>
      {/if}
    </section>

    <section id="Information" style="rgb(254,255,240);
    background: linear-gradient(0deg, rgba(254,255,240,1) 0%, rgba(236,237,223,1) 12%, rgba(220,221,208,1) 21%, rgba(203,204,192,1) 30%, rgba(0,0,0,1) 100%);    
    ">
    </section>

    <section id="Information">
      <div style="    display: flex;
      align-items: center;
      justify-content: center;
      background-color: #FEFFF0;"
      >
        <div id=text-container style=" width: 70%; padding-left: 10%;padding-right: 10%;
        ">
          {#if index >= 5}
          <p > A component in the shuttle’s boosters played a pivotal role in the disaster: the O-Rings.
          <br/>
          <br/>
          O-rings are rubber gaskets used to seal off combustion in the tanks of the solid rocket boosters. If they malfunction, 
          combustion gas releases from the motor and ignites the fuel tank.</p>
          {/if}
        </div>
        <div style="padding-left: 0; padding-right: 10%;">
          <Orings {index}/>  
        </div>
      </div>
    </section>
    
    <section id="Information" style="">
      {#if index >= 6}
      <div id=text-container>
        <p>So were the o-rings defective or is another variable at play? The data collected by the engineers reveals an interesting insight.</p>
      </div>
      {/if}
    </section>

    <section id="Information">
      {#if index >= 7}
        <Trials1 {index}/>
        <div id=text-container> 
          <p>Looking at the launch trials alone, O-rings failed when launch temperatures were both high and low, seemingly suggesting
            that temperature did not play a part in O-ring failure. 
            <br/> 
            <br/> 
            <i style="font-size: 20px">Hover over the shuttles for more information</i>
          </p>
        </div>
      {/if}
    </section>

    <section id="Information" style="padding-top: 10em; height: 110vh;">
      {#if index >= 8}
        <Trials2 {index}/>
        <div id=text-container style="display: block">
          <p style="display: block"> But when the trials are sorted by temperature, there is clear indication that the probability of O-rings failing increases 
            at <br/><mark style="background-color:rgba(0, 0, 0, 0.2);">low temperatures</mark>. </p>  
        </div>
      {/if}
    </section>

    <section id="Information">
      {#if index >= 9}
      <div style="    display: flex;
      align-items: center;
      justify-content: center;
      background-color: #FEFFF0;">
        <div id=text-container style=" width: 70%; padding-left: 10%;padding-right: 10%;
        ">
          <p>The elasticity of O-rings permits air-tight seals. However, at low temperatures O-rings become brittle. This allowed for air 
          to escape which led to the burning of the fuel tank and subsequently, the disaster known today.</p>
        </div>
        <div style="padding-left: 0; padding-right: 10%;">
          <IceOrings {index}/>  
        </div>
      </div>
      {/if}
      </section>

    <section id="Information">
      {#if index >= 10}
      <div id=text-container>
        <p> On the day of the launch, the temperature was 36°F. As a result, the probability of the Challenger exploding due to O-ring 
          failure alone was close to <b>99.9%</b>.</p>
      </div>
      {/if}
    </section>

    <section id="SimTemp">
      {#if index >= 11}
      <SimTemp/>
      {/if}
    </section>

    <section id="Information" style="background-color: #FEFFF0">
      {#if index >= 12}
      <div id=text-container >
        <p>As you run the simulation at 36°F repeatedly, the likelihood of a successful launch for the Challenger is put into perspective, 
          and the consequences of making such a decision. Unlike the simulation, the disaster had real impacts on individuals and their 
          loved ones. So…What can we learn from this?</p>  
      </div>
      {/if}
    </section>

    <section id="Information" style="rgb(254,255,240);
    background: linear-gradient(180deg, rgba(254,255,240,1) 0%, rgba(236,237,223,1) 12%, rgba(220,221,208,1) 21%, rgba(203,204,192,1) 30%, rgba(0,0,0,1) 100%);    
    ">
    </section>

    <section id="Information">
      {#if index >= 14}
      <div id=text-container style="color: #FEFFF0">
        <p>Extensive data analysis from different mediums, including visualization, is crucial to a robust assessment of success likelihood 
          and more importantly safety. Hazards can present themselves at any scale, so it is important to consider risks and take accountability for decisions.
        </p>
      </div>
      {/if}
    </section>

    <section id="Information">
      {#if index >= 15}
      <div id=text-container style="color: #FEFFF0">
        <p>In terms of the series of events leading to the Challenger’s disaster, the company behind the solid rocket boosters, Morton Thiokol, knew of the O-ring 
          issue. Thiokol along with several other key technicians including the engineers voiced their concerns, but the launch continued to take place. Perhaps, if 
          the management at NASA had acknowledged concerns and understood the true likelihood of the Challenger launching successfully, this tragedy could have played out differently.
          <br/>
          <br/>
          <br/>
        </p>  
      </div>
      {/if}
    </section>
  </div>

</Scroller>

</main>

<style>
  :root{
    --theme-color: red
  }
  
  .background {
    width: 100%;
    height: 80vh;
    position: relative;
  }

  .foreground {
    margin: 0 auto;
    height: auto;
    position: relative;
  }

  .progress-bars {
    position: absolute;
    background: rgba(170, 51, 120, 0.2);
    visibility: visible;
  }
  
  section {
    min-height: 90vh;
    text-align: center;
    color: black;
    padding: 0;
  }

  #Title{
    min-height: 90vh;
    font-family: "Open Sans", sans-serif;
    font-size: 20px;

    padding-left: 25%;
    padding-right: 25%;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  #text-container{
    min-height: 90vh;

    font-family: "Open Sans", sans-serif;
    font-size: 23px;
    font-weight: 400;
    line-height:35px;

    padding-left: 25%;
    padding-right: 25%;
    display: flex;
    align-items: center;
    justify-content: center;
    animation: fadein 800ms ease-in;
  }

  #text-container-side{
    min-height: 120vh;
    font-family: "Open Sans", sans-serif;
    font-size: 23px;
    font-weight: 400;
    line-height:35px;

    padding-left: 25%;
    padding-right: 25%;
    display: flex;
    align-items: center;
    justify-content: center;
    animation: fadein 800ms ease-in;
  }



  #SimTemp{
    height: 110vh;
  }

  h1{
    font-family: "Share Tech", sans-serif;
    font-optical-sizing: auto;
    font-weight: 400;
    font-style: normal;
    font-size: 60px;

    color: white;
  }

  :global(body) {
    margin: 0; 
    padding: 0; 
    box-sizing: border-box;
    background-color: black;
    /* var(--theme-color) */
    transition: background-color 0.3 s
  }

  :global(body.light-mode) {
    margin: 0; 
    padding: 0; 
    box-sizing: border-box;
    background-color: #FEFFF0;
    /* var(--theme-color) */
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
