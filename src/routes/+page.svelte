<script lang="ts">
  import { page } from '$app/stores';
  import { onMount } from 'svelte';
  
  let time = $state(new Date());

  let hours = $derived(time.getHours());
  let minutes = $derived(time.getMinutes());
  
  let target1 = new Date();
  target1.setSeconds(0);
  target1.setHours(parseInt($page.url.searchParams.get("h") || "6"));
  target1.setMinutes(parseInt($page.url.searchParams.get("m") || "0"));
  
  let length1 = parseInt($page.url.searchParams.get("t") || "3600") * 1000;
  
  let delta1 = $derived(Math.max(time.getTime() - target1.getTime(), 0));
  let progress1 = $derived(Math.min(delta1 / length1, 1) ** 3);
  
  let target2 = new Date();
  target2.setSeconds(0);
  target2.setHours(parseInt($page.url.searchParams.get("h2") || "20"));
  target2.setMinutes(parseInt($page.url.searchParams.get("m2") || "0"));
  
  let length2 = parseInt($page.url.searchParams.get("t2") || "3600") * 1000;
  
  let delta2 = $derived(Math.max(time.getTime() - target2.getTime(), 0));
  let progress2 = $derived(Math.min(delta2 / length2, 1) ** 3);
  
  let progress = $derived(Math.min(progress1, 1 - progress2))
  
  let randomOffsetX = $state(0);
  let randomOffsetY = $state(0);
  
  let enterpriseMode = $page.url.searchParams.has("enterprise");

  onMount(() => {
    const interval = setInterval(() => {
      time = new Date();
    }, 1000);
    
    const interval2 = setInterval(() => {
      randomOffsetX = (Math.random() - 0.5) * 4;
      randomOffsetY = (Math.random() - 0.5) * 4;
    }, 10000);

    return () => {
      clearInterval(interval);
      clearInterval(interval2);
    };
  });
</script>

<main class={enterpriseMode ? "enterprise" : ""} style="--animation-progress: {progress}; --x: {randomOffsetX}; --y: {randomOffsetY}">
  <h1>
    {hours.toString().padStart(2, '0')}:{minutes.toString().padStart(2, '0')}
  </h1>
</main>

<style>
  main {
    position: relative;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    width: 100vw;
    height: 100vh;

    background: black;
    color: white;
    cursor: none;
    
    transition: filter 1s;
    filter: invert(var(--animation-progress)) sepia(0.2);
    
    &:global(.enterprise)::after {
      content: '';
      z-index: 1;
      position: absolute;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      filter: invert(1);
      background: 100% / 100% 100% url("./ubiquitiman.png");
      opacity: var(--animation-progress)
    }
  }

  p,
  h1 {
    margin: unset;
    padding: unset;
    font-family: system-ui;
  }

  h1 {
    text-align: center;
    font-size: 6em;
    transition: transform linear 120s;
    z-index: 2;
    
    color: rgba(255, 255, 255, clamp(0.1, var(--animation-progress), 1));
    text-shadow: 0.1rem 0.1rem 0.2rem rgba(255, 255, 255, var(--animation-progress));
    transform: translate(calc(var(--x) * 1em), calc(var(--y) * 1em));
  }
</style>
