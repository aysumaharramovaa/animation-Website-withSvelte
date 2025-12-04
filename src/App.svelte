<script>
  import { onMount } from 'svelte';
  import { fly, scale, fade } from 'svelte/transition';

  let scrollY = 0;
  let activeTech = null;

const techs = [
  { 
    name: "HTML5", 
    img: "https://cdn-icons-png.flaticon.com/512/732/732212.png", 
    desc: "HTML5 is the main language used to structure web pages." 
  },
  { 
    name: "CSS3", 
    img: "https://cdn-icons-png.flaticon.com/512/732/732190.png", 
    desc: "CSS3 is used to style web pages and create animations." 
  },
  { 
    name: "JavaScript", 
    img: "https://cdn-icons-png.flaticon.com/512/5968/5968292.png", 
    desc: "JavaScript adds interactivity to web pages." 
  },
  { 
    name: "Svelte", 
    img: "https://upload.wikimedia.org/wikipedia/commons/1/1b/Svelte_Logo.svg", 
    desc: "Svelte is a component-based framework that compiles at build time." 
  }
];

  let scales = techs.map(() => 1);

  const handleScroll = () => {
    scrollY = window.scrollY;
    scales = scales.map((s, i) => {
      let newScale = 1 + scrollY / 1000 - i * 0.05;
      if (newScale < 0.6) newScale = 0.6;
      if (newScale > 1.5) newScale = 1.5;
      return newScale;
    });
  }

  const openTech = (tech) => {
    activeTech = tech;
  }

  const closeTech = () => {
    activeTech = null;
  }

  onMount(() => {
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  });
</script>

<style>
  body {
    margin: 0;
    font-family: 'Segoe UI', sans-serif;
    background: radial-gradient(circle at top, #111 0%, #000 100%);
    color: #fff;
    overflow-x: hidden;
  }

  .container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 50px;
    padding: 80px 20px;
    position: relative;
  }

  .tech-card {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    width: 180px;
    padding: 25px;
    border-radius: 20px;
    background: rgba(255,255,255,0.05);
    border: 2px solid rgba(255,255,255,0.2);
    backdrop-filter: blur(8px);
    cursor: pointer;
    transition: transform 0.4s, box-shadow 0.4s, border-color 0.3s;
  }

  .tech-card:hover {
    transform: scale(1.1);
    box-shadow: 0 0 20px #00ffff, 0 0 30px #587878;
    border-color: #00ffff;
  }

  .tech-card img {
    width: 100px;
    height: 100px;
    margin-bottom: 15px;
    transition: transform 0.3s;
    filter: drop-shadow(0 0 5px #00ffff);
  }

  h2 {
    margin: 0 0 10px 0;
    font-size: 1.2rem;
  }

  p {
    font-size: 0.85rem;
    color: #aaa;
  }

  .modal-bg {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    background: rgba(0,0,0,0.85);
    display: flex; justify-content: center; align-items: center;
    z-index: 10;
  }

  .modal {
    background: #111;
    padding: 30px;
    border-radius: 20px;
    max-width: 500px;
    width: 90%;
    text-align: center;
    box-shadow: 0 0 30px #00ffff;
    border: 2px solid #00ffff;
  }

  .modal img {
    width: 120px;
    height: 120px;
    margin-bottom: 20px;
    filter: drop-shadow(0 0 10px #00ffff);
  }

  .modal h2 {
    margin-bottom: 15px;
    font-size: 1.6rem;
    color: #00ffff;
  }

  .modal p {
    font-size: 1rem;
    color: #ccc;
  }

  .close-btn {
    margin-top: 20px;
    padding: 12px 30px;
    background: #00ffff;
    color: #000;
    border: none;
    border-radius: 10px;
    cursor: pointer;
    font-weight: bold;
    transition: transform 0.2s, background 0.2s;
  }

  .close-btn:hover {
    transform: scale(1.05);
    background: #67b3b3;
  }
</style>

<div class="container">
  {#each techs as tech, i (tech.name)}
    <div class="tech-card" style="transform: scale({scales[i]});" on:click={() => openTech(tech)}
         in:fly={{ y: 100, duration: 700, delay: i * 150 }}
         out:fade={{ duration: 200 }}>
      <img src={tech.img} alt={tech.name} />
      <h2>{tech.name}</h2>
      <p>Click for more info</p>
    </div>
  {/each}
</div>

{#if activeTech}
  <div class="modal-bg" on:click={closeTech} transition:fade>
    <div class="modal" on:click|stopPropagation transition:scale={{ duration: 300 }}>
      <img src={activeTech.img} alt={activeTech.name} />
      <h2>{activeTech.name}</h2>
      <p>{activeTech.desc}</p>
      <button class="close-btn" on:click={closeTech}>Close</button>
    </div>
  </div>
{/if}
