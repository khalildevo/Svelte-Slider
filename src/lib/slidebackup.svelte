<script>
  import { blur } from "svelte/transition";
  export let slides;
  export let duration = 2000;

  let slide = 0;
  let slideNext = () => {
    slide++;
    if (slide >= slides.length) {
      slide = 0;
    }
    timer();
  };
  let slidePrev = () => {
    slide--;
    if (slide <= -1) {
      slide = slides.length - 1;
    }
    timer();
  };

  let goToSlide = (i) => {
    slide = i;
    timer();
  };
  let interval;
  const timer = () => {
    clearInterval(interval);
    interval = setInterval(slideNext, duration);
  };
  timer();
</script>

<div class="slider">
  {#each slides as slider, i}
    {#if slide === i}
      <div class="slide" transition:blur={{ amount: 20 }}>
        {slider}
      </div>
    {/if}
  {/each}
  <button class="prev" on:click={slidePrev}>Prev</button>
  <button class="next" on:click={slideNext}>Next</button>
  <div class="nav">
    {#each slides as slider, i}
      <button
        on:click={() => goToSlide(i)}
        class="bubble"
        class:active={i === slide}
        class:sp={i === slide - 1}
        class:sn={i === slide + 1}
        class:twoback={i === slide - 2}
        class:twofront={i === slide + 2}
      />
    {/each}
  </div>
</div>

<style>
  .slider {
    position: relative;
    width: 100vw;
    height: 100vh;
    background-color: #000;
  }
  .slide {
    position: absolute;
    inset: 0;
    color: #fff;
    font-size: 7rem;
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: Arial, Helvetica, sans-serif;
  }
  .next,
  .prev {
    position: absolute;
    z-index: 2;
    top: 50%;
    transform: translateY(-50%);
    border: 1px solid #fff;
    background: transparent;
    border-radius: 6px;
    color: #fff;
    cursor: pointer;
  }
  .next {
    right: 20px;
  }
  .prev {
    left: 20px;
  }

  .nav {
    position: absolute;
    bottom: 20px;
    left: 0;
    right: 0;
    display: flex;
    justify-content: space-evenly;
    align-items: center;
    height: 100px;
  }
  .bubble {
    height: 20px;
    border-radius: 400px;
    opacity: 0.2;
    transition: all 0.5s ease-out;
    cursor: pointer;
  }
  .bubble:hover {
    opacity: 0.8;
  }
  .active {
    height: 80px;
    opacity: 1;
  }
  .sp,
  .sn {
    height: 60px;
  }
  .twoback,
  .twofront {
    height: 40px;
  }
</style>
