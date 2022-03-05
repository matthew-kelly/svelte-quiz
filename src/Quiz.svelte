<script>
  import { fade, blur, fly, slide, scale } from "svelte/transition";
  import { onMount, beforeUpdate, afterUpdate, onDestroy } from "svelte";
  import Question from "./Question.svelte";
  import Modal from "./Modal.svelte";
  import { score } from "./store";

  let isModalOpen = false;
  let quizLength = 10;
  let activeQuestion = 0;
  let quiz = getQuiz();

  // onMount(() => {
  //   console.log("onMount");
  // });
  // beforeUpdate(() => {
  //   console.log("before update");
  // });
  // afterUpdate(() => {
  //   console.log("after update");
  // });

  async function getQuiz() {
    const res = await fetch(
      `https://opentdb.com/api.php?amount=${quizLength}&type=multiple`
    );
    return res.json();
  }

  function nextQuestion() {
    activeQuestion += 1;
  }

  function resetQuiz() {
    isModalOpen = false;
    score.set(0);
    activeQuestion = 0;
    quiz = getQuiz();
  }

  // reactive statement
  $: if (activeQuestion >= quizLength) {
    isModalOpen = true;
  }

  // reactive declaration
  $: questionNumber = activeQuestion + 1;
</script>

<div>
  <button on:click={resetQuiz}>Start New Quiz</button>

  <h3>Score: {$score}</h3>
  <h4>Question #{questionNumber}</h4>

  {#await quiz}
    <h3>Loading...</h3>
  {:then data}
    {#each data.results as question, index}
      {#if index === activeQuestion}
        <div in:fly={{ x: -100 }} out:fly={{ x: 100 }} class="fade-wrapper">
          <Question {question} {nextQuestion} />
        </div>
      {/if}
    {/each}
  {/await}
</div>

{#if isModalOpen}
  <Modal on:close={resetQuiz}>
    <h2>Quiz completed!</h2>
    <h3>Final Score: {$score}/{quizLength}</h3>
    <button on:click={resetQuiz}>Start a New Quiz</button>
  </Modal>
{/if}

<style>
  .fade-wrapper {
    position: absolute;
  }
</style>
