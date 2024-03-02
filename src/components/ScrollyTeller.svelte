<script>
    import Scroller from "@sveltejs/svelte-scroller";
    import SimTemp from "./SimTemp.svelte";
    import { geoMercator } from "d3-geo";

    let count, index, offset, progress;
    let width, height;

    let geoJsonToFit = {
    type: "FeatureCollection",
    features: [
      {
        type: "Feature",
        geometry: {
          type: "Point",
          coordinates: [1, 0],
        },
      },
      {
        type: "Feature",
        geometry: {
          type: "Point",
          coordinates: [0, 1],
        },
      },
    ],
  };

  $: projection = geoMercator().fitSize([width, height], geoJsonToFit);
</script>

<Scroller
  top={0.0}
  bottom={1}
  threshold={0.5}
  bind:count
  bind:index
  bind:offset
  bind:progress
>
  <!-- <div 
  class="background" 
  slot="background"
  bind:clientWidth={width}
  bind:clientHeight={height}
  >

    <div class="progress-bars">
      <p>Current Section: <strong>{index + 1}/{count}</strong></p>
      <progress value={count ? (index + 1) / count : 0} />

      <p>Offset in current section</p>
      <progress value={offset || 0} />

      <p>Total Progress</p>
      <progress value={progress || 0} />
    </div>

  </div> -->

  <div class="foreground" slot="foreground">
    <section id="first">
      <SimTemp {index} />
    </section>
    <section> 
      <h1> Implemented Aspects</h1> 
      <p>
          We began our project by deciding on the topic of the Space Shuttle Challenger disaster and choosing a dataset alongside it. Our dataset provides recorded performance metrics and temperature conditions of the environment during trials of the Challenger. After this, we planned out the order and general idea for each section of our scrolling website. We then implemented a simulation that illustrates the likelihood of a space shuttle explosion in relation to the environment’s temperature. We used the recorded number of o-rings with thermal stress from the data to fit a logistic regression model that classifies whether the shuttle launched or failed. This is due to the relation between o-ring functionality and temperature, and how the rings fail to work properly at lower temperatures. The viewers can engage with the simulation by choosing a particular temperature with the slider and then launching the shuttle with the press of a button to determine if the launch will be successful. 
      </p>

      <h1> Future Challenges</h1>
      <p style="padding-bottom: 50px"> 
      The most challenging part of our project going forward would be implementing smooth transitions between sections. This is because we plan to add multiple elements within each section and style some sections contrastingly. This would potentially cause formatting issues, especially with the positioning. Additionally, we must accommodate for different viewing experiences across various screen and window sizes. Hence, because of this variability, styling could possibly be a long process as bridging all aspects in a seamless manner poses a challenge.  
      </p> 
    </section>
  </div>

</Scroller>

<style>
  .background {
    width: 100%;
    height: 100%;
    position: relative;
  }

  .foreground {
    width: 50%;
    margin: 0 auto;
    height: auto;
    position: relative;
  }

  /* .progress-bars {
    position: absolute;
    background: rgba(170, 51, 120, 0.2);
    visibility: visible;
  } */
  
  section {
    height: 80vh;
    color: white;
    text-align: center;
    color: black;
    padding: 1em;
    margin: 0 0 2em 0;
  }

  #first{
    height: 110vh;
  }

  p{
    /* font-size: medium; */
    text-indent: 40px;
    font-family: "Arial", arial;
    font-weight: 300;
    text-align: left;
    line-height:25px
  }
</style>
