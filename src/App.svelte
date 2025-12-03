<script>
  let name = "Aysu";
  let count = 0;

  function increment() {
    count += 1;
  }
</script>

<main>
  <h1>Hello {name}!</h1>
  <p>Button clicked {count} times.</p>
  <button on:click={increment}>Click me</button>
</main>

<style>
  main {
    text-align: center;
    margin-top: 50px;
    font-family: sans-serif;
  }

  button {
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
  }
</style>
