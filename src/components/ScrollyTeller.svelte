<script>
    import { onMount } from "svelte";
    import Scroller from "@sveltejs/svelte-scroller";
    import SimTemp from "./SimTemp.svelte";
    import Trials1 from "./Trials1.svelte";
    import Trials2 from "./Trials2.svelte";
    import Orings from "./Orings.svelte";
    import IceOrings from "./IceOrings.svelte";
    import Introduction from "./Introduction.svelte";   
    import Conclusion from "./Conclusion.svelte";
    import Loading from "./Loading.svelte";
    import * as d3 from 'd3';

    let count, index, offset, progress;
    let width, height;
    let mode = false;
    let loaded = false;

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
      <div style="color:white;">
        <h1 style=""> WHAT WERE THE ODDS OF THE CHALLENGER DISASTER? </h1>
        By: Cristina De La Torre and Kathleen Nguyen

        <Loading {index}/>
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
      <div style="    
      display: flex;
      align-items: center;
      justify-content: center;
      background-color: #FEFFF0;"
      >
      {#if index >= 5}
      <div id=text-container style="width: 70%; padding-left: 10%;padding-right: 0%;
        ">
          <p > A component in the shuttle’s boosters played a pivotal role in the disaster: the O-Rings.
          <br/>
          <br/>
          O-rings are rubber gaskets used to seal off combustion in the tanks of the solid rocket boosters. If they malfunction, 
          combustion gas releases from the motor and ignites the fuel tank.</p>
        </div>
        <div id=text-container style="padding-left: 0; padding-right: 0%;">
          <Orings {index}/>  
        </div>
        {/if}
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
          <p>
            <br/> 
            <br/> 
            <br/> 
            Looking at the launch trials alone, O-rings failed when launch temperatures were both high and low, seemingly suggesting
            that temperature did not play a part in O-ring failure. 
            <br/> 
            <br/> 
            <br/> 
            <br/> 
            <br/> 
            <br/> 
            <i style="font-size: 20px">Hover over the shuttles for more information</i>
            <br/> 
            <span style="color: black; font-size: 20px"> <b>black</b> </span> = successful launch
            <br/> 
            <span style="color: red; font-size: 20px"> <b>red</b> </span> = failed launch
          </p>
        </div>
      {/if}
    </section>

    <section id="Information" style="padding-top: 10em; height: 110vh;">
      {#if index >= 8}
        <Trials2 {index}/>
        <div id=text-container style="display: block">
          <p style="display: block"> But when the trials are sorted by temperature, there is clear indication that the probability of O-rings failing increases 
            at <br/><mark style="background-color: #DAEDFB;">low temperatures</mark>. </p>  
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
        <div id=text-container  style="padding-left: 0; padding-right: 10%;">
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

    <section id="Information" style="background-color: #FEFFF0;">
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

    <section id="Information" style="margin-bottom: 0px">
      {#if index >= 15}
      <div id=text-container style="color: #FEFFF0;display: block; padding-left: 15%; padding-right: 15%; min-height: 0vh;">
        <p>In terms of the series of events leading to the Challenger’s disaster, the company behind the solid rocket boosters, Morton Thiokol, knew of the O-ring 
          issue. Thiokol along with several other key technicians including the engineers voiced their concerns, but the launch continued to take place. Perhaps, if 
          the management at NASA had acknowledged concerns and understood the true likelihood of the Challenger launching successfully, this tragedy could have played out differently.
          <br/>
          <br/>
          <br/>
        </p>  
        <Conclusion {index}/>
        <p style="font-size:16px; padding-left: 15%; padding-right: 15%">
          <i>The crew members at the White Room during countdown training. Left to right: Sharon Christa McAuliffe, Gregory Jarvis, Judith A. Resnik, Francis R. (Dick) Scobee, Ronald E. McNair,
          Mike J. Smith, and Ellison S. Onizuka(9 Jan. 1986)</i>
        </p>
      </div>
      {/if}
    </section>

    <section id="Information" style="margin-bottom: 0px; min-height: 100vh">
      {#if index >= 16}
      <div id="text-container-left" style="color: #FEFFF0; text-align: left; padding-left: 25%; padding-top: 10%; font-size: 16px">
        <h1 style="display: block; font-size: 50px"> METHODS</h1>
        <p>
          To analyze the odds of the Challenger successfully launching, we researched the narrative behind the disaster and the space 
          shuttle’s trial data. We use a dataset on performance metrics of the Challenger’s trials from UCI’s Machine Learning 
          Repository. The dataset we use is “Primary O-ring Erosion and/or Blowby” which includes information on the number of 
          O-rings at risk on a given flight, the number experiencing thermal distress, the launch temperature in degrees Fahrenheit, 
          leak-check pressure in psi, and the temporal order of flight.
          <br/>
          <br/>
          We then referenced statistical analysis on the Challenger’s trials, o-ring damage, and its relationship to temperature. By 
          doing so we found that the probability of o-ring damage in relation to the temperature during launch is described by a logistic
          model. Using Scikit-learn we fit a logistic regression model to trial data to predict probability of o-ring damage given launch 
          temperature. We define damaged o-rings as those that experienced thermal stress during a trial. Along with this, we describe a 
          successful launch to be one such that there were no o-rings that experienced thermal stress and label launches unsuccessful otherwise. 
          This is because when evaluating launch success solely on o-rings and temperature, o-ring distress poses a risk to blow-by gasses. 
          We then implemented this within our simulation of the Challenger’s launch at different temperatures including the actual launch 
          temperature. For more information on our motivations and objectives watch our overview 
          <a href= "https://www.youtube.com/watch?v=2bBDswzQooI"> video.</a> For more on implemented visualizations and our site’s documentation 
          visit our <a  href= "https://github.com/knguyen64/challenger-exploration"> Github repository.</a>
        </p>  
      </div>
      {/if}
    </section>

    <section id="Information" style="margin-bottom: 0px; min-height: 100vh">
      {#if index >= 17}
      <div id="text-container-left" style="color: #FEFFF0; text-align: left; padding-left: 25%; padding-top: 5%; font-size: 16px">
        <h1 style="display: block; font-size: 50px"> SOURCES</h1>
        <p>
          “A Statistical Analysis of the Challenger Accident.” A Statistical Analysis of the Challenger Accident, <a href= "https://blog.rebellionresearch.com/blog/a-statistical-analysis-of-the-challenger-accident" 
          > blog.rebellionresearch.com/blog/a-statistical-analysis-of-the-challenger-accident.</a>
          <br/>
          <br/>

          “Cape Canaveral 1986 Past Weather (Florida, United States) - Weather Spark.” Weather Spark, <a href= "https://weatherspark.com/h/y/18782/1986/Historical-Weather-during-1986-in-Cape-Canaveral-Florida-United-States " > weatherspark.com/h/y/18782/1986/Historical-Weather-during-1986-in-Cape-Canaveral-Florida-United-States.
          </a>
          <br/>
          <br/>

          Challenger O-Ring Data – Logistic Regression. <a href= "https://byuistats.github.io/Statistics-Notebook/Analyses/Logistic%20Regression/Examples/challengerLogisticReg.html#" > byuistats.github.io/Statistics-Notebook/Analyses/Logistic%20Regression/Examples/challengerLogisticReg.html#.</a>
          <br/>
          <br/>

          “Challenger STS-51L Accident - NASA.” NASA, <a href= "https://www.nasa.gov/challenger-sts-51l-accident/#documents">www.nasa.gov/challenger-sts-51l-accident/#documents.</a>
          <br/>
          <br/>

          CNN. “Space Shuttle Challenger Explosion (1986).” YouTube, 27 Jan. 2011, <a href= "https://www.youtube.com/watch?v=AfnvFnzs91s">www.youtube.com/watch?v=AfnvFnzs91s.</a>
          <br/>
          <br/>

          OpenCourseWare, MIT. “Solid Rocket Booster Design Showing Location of O-Rings.” Flickr, <a href= "http://www.flickr.com/photos/mitopencourseware/4011768300/in/photostream">www.flickr.com/photos/mitopencourseware/4011768300/in/photostream.</a>
          <br/>
          <br/>

          The Space Shuttle Challenger Disaster | Online Ethics. 1992,  <a href= "https://onlineethics.org/cases/engineering-ethics-cases-texas-am/space-shuttle-challenger-disaster">onlineethics.org/cases/engineering-ethics-cases-texas-am/space-shuttle-challenger-disaster.</a>
          <br/>
          <br/>

          UCI Machine Learning Repository. <a href= "https://archive.ics.uci.edu/dataset/92/challenger+usa+space+shuttle+o+ring">archive.ics.uci.edu/dataset/92/challenger+usa+space+shuttle+o+ring.</a>
          <br/>
          <br/>

          V1ch3.  <a href= "https://www.nasa.gov/history/rogersrep/v1ch3.htm">www.nasa.gov/history/rogersrep/v1ch3.htm.</a>
          <br/>
          <br/>

          Wikipedia contributors. File:Shuttle.png - Wikipedia.<a href= "https://en.m.wikipedia.org/wiki/File:Shuttle.png">en.m.wikipedia.org/wiki/File:Shuttle.png.</a>
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

  #text-container-left{
    min-height: 90vh;
    font-family: "Open Sans", sans-serif;
    font-size: 23px;
    font-weight: 400;
    line-height:35px;

    padding-left: 25%;
    padding-right: 25%;
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

  .arrow {
    border: solid white;
    border-width: 0 3px 3px 0;
    display: inline-block;
    padding: 15px;
  }

  .down {
    transform: rotate(45deg);
    -webkit-transform: rotate(45deg);
  }

  a{
    color: #FF9549;
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
