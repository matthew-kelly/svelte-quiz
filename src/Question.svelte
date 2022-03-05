<script>
  import { score } from "./store";

  export let question;
  export let nextQuestion;

  let isCorrect, isAnswered;
  let answers = question.incorrect_answers.map((answer) => ({
    answer,
    correct: false,
  }));
  let allAnswers = [
    ...answers,
    { answer: question.correct_answer, correct: true },
  ];
  shuffle(allAnswers);

  function shuffle(array) {
    array.sort(() => Math.random() - 0.5);
  }

  function checkQuestion(correct) {
    if (!isAnswered) {
      isAnswered = true;
      isCorrect = correct;
      if (correct) {
        score.update((prev) => prev + 1);
      }
    }
  }
</script>

<div>
  <h3>{@html question.question}</h3>

  {#if isAnswered}
    <h5 class={isCorrect ? "correct" : "incorrect"}>
      {#if isCorrect}
        You got it right!
      {:else}
        You goofed up!
      {/if}
    </h5>
  {/if}

  {#if isAnswered}
    <h5 class="correct">{@html question.correct_answer}</h5>
    <button on:click={nextQuestion}>Next</button>
  {:else}
    {#each allAnswers as answer}
      <button
        disabled={isAnswered}
        on:click={() => checkQuestion(answer.correct)}
      >
        {@html answer.answer}
      </button>
    {/each}
  {/if}
</div>

<style>
  .correct {
    color: green;
  }
  .incorrect {
    color: red;
  }
</style>
