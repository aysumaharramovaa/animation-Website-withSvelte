<script>
  import { onMount } from 'svelte';

  let scrollY = 0;
  let activeTech = null; // info 

  const techs = [
    { name: "HTML5", img: "https://cdn-icons-png.flaticon.com/512/732/732212.png", desc: "HTML5 veb səhifələrin strukturunu yaradan əsas dildir." },
    { name: "CSS3", img: "https://cdn-icons-png.flaticon.com/512/732/732190.png", desc: "CSS3 saytların dizaynını və animasiyalarını təyin edir." },
    { name: "JavaScript", img: "https://cdn-icons-png.flaticon.com/512/5968/5968292.png", desc: "JavaScript veb səhifələrə interaktivlik əlavə edir." },
    { name: "Svelte", img: "https://upload.wikimedia.org/wikipedia/commons/1/1b/Svelte_Logo.svg", desc: "Svelte komponent əsaslı frameworkdür və compile zamanı işləyir." }
  ];

  let scales = techs.map(() => 1);

  const handleScroll = () => {
    scrollY = window.scrollY;
    scales = scales.map((s, i) => {
      let newScale = 1 + scrollY / 800 - i * 0.05;
      if (newScale < 0.5) newScale = 0.5;
      if (newScale > 2) newScale = 2;
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
    font-family: sans-serif;
    background: #f4f4f9;
  }

  .container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 50px;
    padding: 50px 20px;
  }

  .tech-card {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    width: 180px;
    padding: 20px;
    border-radius: 20px;
    background: white;
    box-shadow: 0 10px 20px rgba(0,0,0,0.1);
    transition: transform 0.3s, box-shadow 0.3s;
    cursor: pointer;
  }

  .tech-card:hover {
    box-shadow: 0 15px 25px rgba(0,0,0,0.2);
  }

  .tech-card img {
    width: 100px;
    height: 100px;
    margin-bottom: 15px;
    transition: transform 0.3s;
  }

  h2 {
    margin: 0 0 10px 0;
    font-size: 1.1rem;
  }

  p {
    font-size: 0.85rem;
    color: #555;
  }

  /* Modal */
  .modal-bg {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.6);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 10;
  }

  .modal {
    background: white;
    padding: 30px;
    border-radius: 20px;
    max-width: 500px;
    width: 90%;
    text-align: center;
    box-shadow: 0 10px 30px rgba(0,0,0,0.3);
    animation: fadeIn 0.3s ease-out;
  }

  .modal img {
    width: 120px;
    height: 120px;
    margin-bottom: 20px;
  }

  .modal h2 {
    margin-bottom: 15px;
    font-size: 1.5rem;
  }

  .modal p {
    font-size: 1rem;
    color: #444;
  }

  .close-btn {
    margin-top: 20px;
    padding: 10px 25px;
    background: #007bff;
    color: white;
    border: none;
    border-radius: 10px;
    cursor: pointer;
    transition: background 0.2s;
  }

  .close-btn:hover {
    background: #0056b3;
  }

  @keyframes fadeIn {
    from {opacity: 0; transform: scale(0.9);}
    to {opacity: 1; transform: scale(1);}
  }
</style>

<div class="container">
  {#each techs as tech, i}
    <div class="tech-card" style="transform: scale({scales[i]});" on:click={() => openTech(tech)}>
      <img src={tech.img} alt={tech.name} />
      <h2>{tech.name}</h2>
      <p>Click for more info</p>
    </div>
  {/each}
</div>

{#if activeTech}
  <div class="modal-bg" on:click={closeTech}>
    <div class="modal" on:click|stopPropagation>
      <img src={activeTech.img} alt={activeTech.name} />
      <h2>{activeTech.name}</h2>
      <p>{activeTech.desc}</p>
      <button class="close-btn" on:click={closeTech}>Close</button>
    </div>
  </div>
{/if}
