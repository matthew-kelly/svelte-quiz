<script>
  import Quiz from "./Quiz.svelte";
</script>

<main>
  <Quiz />
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
    position: relative;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
