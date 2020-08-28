<script>
  import { onMount } from "svelte";
  import { tweened } from "svelte/motion";
  import { cubicOut } from "svelte/easing";
  import { fade, fly } from "svelte/transition";
  import LottieAnimation from "./LottieAnimation.svelte";
  import LottieCorrect from "./LottieCorrect.svelte";
  export let addScore;
  export let trivia;
  export let answers;
  export let score;
  export let toggleForm;
  export let toggleLoader;
  export let toggleFormSubmitted;
  let currentQuestion = 0;
  let showCorrectAnimation = false;
  let showFalseAnimation = false;
  let visible = true;
  let showScore = false;

  const progress = tweened(0, {
    duration: 400,
    easing: cubicOut,
  });

  const updateProgress = (currentQuestion, totalQuestions) => {
    const progressVal = currentQuestion / totalQuestions;
    progress.set(progressVal);
  };

  const startNewGame = () => {
    toggleForm();
    toggleFormSubmitted();
    toggleLoader();
    showScore = false;
  };

  const showCorrect = () => {
    showCorrectAnimation = true;
    setTimeout(() => {
      showCorrectAnimation = false;
    }, 1000);
  };

  const showFalse = () => {
    showFalseAnimation = true;
    setTimeout(() => {
      showFalseAnimation = false;
    }, 1000);
  };

  const sanitizeText = (text) => {
    const editedText = text.replace(/&#039;/g, "'");
    return editedText.replace(/(&quot\;)/g, '"');
  };

  const handleSubmit = (e, index) => {
    console.log(e.target.value);
    const userResponse = e.target.value;
    currentQuestion += 1;
    updateProgress(index + 1, trivia.length);
    if (userResponse === trivia[index].correct_answer) {
      addScore();
      showCorrect();
    } else {
      showFalse();
    }
    if (index + 1 === trivia.length) {
      showScore = true;
    }
    console.log(index);
  };
</script>

<style>
  .grid {
    margin-top: 3rem;
    display: grid;
    grid-gap: 2rem;
    grid-template-columns: 1fr;
  }
  progress {
    display: block;
    width: 100%;
    color: rgba(255, 255, 255, 0.1);
  }
  .icon {
    text-align: center;
    margin: 0 auto;
    font-size: 4rem;
  }
  .card {
    border: solid rgba(255, 255, 255, 0.1) 2px;
    border-radius: 2rem;
    padding: 3rem 2rem;
    transition: 500ms ease-in-out;
    position: absolute;
    margin-left: auto;
    margin-right: auto;
    left: 0;
    right: 0;
    text-align: center;
    width: 90%;
    max-width: 500px;
    overflow: hidden;
  }

  @media (min-width: 576px) {
  }

  @media (min-width: 768px) {
  }

  @media (min-width: 992px) {
  }
</style>

<div>
  {#if !showScore}
    <progress value={$progress} />
  {/if}
  {#if showFalseAnimation}
    <LottieAnimation />
  {/if}
  {#if showCorrectAnimation}
    <LottieCorrect />
  {/if}
  <div class="grid">

    {#if showScore}
      <div class="card">
        <h2>Congratulations on finishing your test</h2>
        <p>You scored {score} / {trivia.length}</p>
        <button on:click={startNewGame}>New Game</button>
      </div>
    {/if}
    {#each trivia as card, i}
      {#if currentQuestion === i}
        <div
          in:fly={{ y: 200, duration: 1000 }}
          out:fly={{ y: -100, duration: 1000 }}
          class="card">
          <p>
            <strong>{i + 1}.</strong>
            {sanitizeText(card.question)}
          </p>
          <form on:change={(e) => handleSubmit(e, i)}>
            <input
              type="radio"
              id={answers[i][0]}
              name="gender"
              value={answers[i][0]} />
            <label for={answers[i][0]}>{sanitizeText(answers[i][0])}</label>
            <br />
            <input
              type="radio"
              id={answers[i][1]}
              name="gender"
              value={answers[i][1]} />
            <label for={answers[i][1]}>{sanitizeText(answers[i][1])}</label>
            <br />
            <input
              type="radio"
              id={answers[i][2]}
              name="gender"
              value={answers[i][2]} />
            <label for={answers[i][2]}>{sanitizeText(answers[i][2])}</label>
            <input
              type="radio"
              id={answers[i][3]}
              name="gender"
              value={answers[i][3]} />
            <label for={answers[i][3]}>{sanitizeText(answers[i][3])}</label>
          </form>
        </div>
      {/if}
    {/each}
  </div>
</div>
