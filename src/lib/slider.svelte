<script>
  import { blur } from "svelte/transition";

  export let duration;
  export let slides;
  let currentSlide = 0;
  const nextSlide = () => {
    currentSlide++;
    if (currentSlide >= slides.length) {
      currentSlide = 0;
    }
    timer();
  };
  const prevSlide = () => {
    currentSlide--;
    if (currentSlide <= 0) {
      currentSlide = slides.length - 1;
    }
    timer();
  };
  const goToSlide = (i) => {
    currentSlide = i;
    timer();
  };
  let interval;
  const timer = () => {
    clearInterval(interval);
    interval = setInterval(nextSlide, duration);
  };
  timer();
</script>

<div class="slider">
  {#each slides as slider, i}
    {#if currentSlide === i}
      <div class="slide" transition:blur={{ amount: 10 }}>
        {slider}
      </div>
    {/if}
  {/each}
  <button class="next" on:click={nextSlide}>Next</button>
  <button class="prev" on:click={prevSlide}>Prev</button>
  <div class="nav">
    {#each slides as slider, i}
      <button
        class="bubble"
        on:click={() => {
          goToSlide(i);
        }}
        class:current={i === currentSlide}
        class:onedown={i === currentSlide - 1}
        class:twodown={i === currentSlide - 2}
        class:oneup={i === currentSlide + 1}
        class:twoup={i === currentSlide + 2}
      />
    {/each}
  </div>
</div>

<style>
  .slider {
    width: 100vw;
    height: 100vh;
    position: relative;
    background-color: #222;
  }
  .slide {
    position: absolute;
    inset: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #fff;
    font-size: 7rem;
    font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI",
      Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue",
      sans-serif;
    font-weight: 200;
  }
  .next,
  .prev {
    position: absolute;
    z-index: 2;
    top: 50%;
    transform: translateY(-50%);
    background: transparent;
    border: 1px solid #fff;
    color: #fff;
    padding: 4px 10px;
    border-radius: 4px;
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
    height: 100px;
    z-index: 3;
    display: flex;
    justify-content: space-evenly;
    align-items: center;
  }
  .bubble {
    padding: 0;
    border: 0;
    height: 20px;
    width: 10px;
    border-radius: 100px;
    transition: all 0.3s ease-out;
    opacity: 0.2;
    cursor: pointer;
  }
  .current {
    height: 80px;
    opacity: 0.8;
  }
  .onedown,
  .oneup {
    height: 60px;
  }
  .twodown,
  .twoup {
    height: 40px;
  }
</style>
