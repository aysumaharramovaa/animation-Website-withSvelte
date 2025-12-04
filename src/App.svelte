<script>
  import { onMount } from 'svelte';

  let scrollY = 0;

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

  onMount(() => {
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  });
</script>

<style>
  body { margin: 0; font-family: sans-serif; background: #f4f4f9; }
  .container { display: flex; flex-direction: column; align-items: center; padding: 50px 20px; gap: 100px; }
  .tech-card { display: flex; flex-direction: column; align-items: center; text-align: center; max-width: 300px; padding: 20px; border-radius: 20px; background: white; box-shadow: 0 10px 20px rgba(0,0,0,0.1); transition: transform 0.3s; }
  .tech-card img { width: 100px; height: 100px; margin-bottom: 20px; transition: transform 0.3s; }
  h2 { margin: 0 0 10px 0; }
  p { font-size: 0.9rem; color: #555; }
</style>

<div class="container">
  {#each techs as tech, i}
    <div class="tech-card" style="transform: scale({scales[i]});">
      <img src={tech.img} alt={tech.name} />
      <h2>{tech.name}</h2>
      <p>{tech.desc}</p>
    </div>
  {/each}
</div>
