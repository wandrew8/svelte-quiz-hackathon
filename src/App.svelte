<script>
  import TriviaCard from "./components/TriviaCard.svelte";
  import Icon from "svelte-awesome";
  import { spinner } from "svelte-awesome/icons";
  let formSubmitted = false;
  let isLoading = false;
  let difficulty;
  let numQuestions;
  let category;
  let score = 0;
  let showForm = true;
  let trivia = [];
  let answers = [];

  const getTrivia = async () => {
    trivia = [];
    answers = [];
    isLoading = true;
    const res = await fetch(
      `https://opentdb.com/api.php?amount=${numQuestions}&category=${category}&difficulty=${difficulty}&type=multiple`
    );
    const data = await res.json();
    trivia = data.results;
    getQuestions(trivia);
    toggleLoader();
    console.log(trivia);
    console.log(answers);
  };

  const getQuestions = async (questions) => {
    const array = await questions.forEach((question) => {
      let answersArray = question.incorrect_answers;
      answersArray.push(question.correct_answer);
      const randomizedArray = shuffle(answersArray);
      answers.push(randomizedArray);
    });
    if (array) {
      return array.forEach((answer) => {
        sanitizeText(answer);
      });
    }
  };

  const shuffle = (array) => {
    let currentIndex = array.length,
      temporaryValue,
      randomIndex;
    while (0 !== currentIndex) {
      randomIndex = Math.floor(Math.random() * currentIndex);
      currentIndex -= 1;

      temporaryValue = array[currentIndex];
      array[currentIndex] = array[randomIndex];
      array[randomIndex] = temporaryValue;
    }
    return array;
  };

  const sanitizeText = (text) => {
    const editedText = text.replace(/&#039;/g, "'");
    return editedText.replace(/(&quot\;)/g, '"');
  };

  const toggleLoader = () => {
    isLoading = false;
  };

  const addScore = () => {
    score += 1;
  };

  const toggleForm = () => {
    showForm = true;
  };

  const toggleFormSubmitted = () => {
    formSubmitted = false;
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    getTrivia();
    formSubmitted = false;
    formSubmitted = true;
    showForm = false;
    console.log(difficulty, numQuestions, category);
  };
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    margin: 0 auto;
  }
  select {
    margin-bottom: 1rem;
  }
  h1 {
    color: #1abc9c;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }
  select {
    display: inline;
    margin-right: 0.1rem;
    padding: 1rem;
  }
  button {
    background-color: #1abc9c;
  }
  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  <h1>Trivia Game</h1>
  {#if showForm}
    <form on:submit={handleSubmit}>
      <select bind:value={difficulty}>
        <option value="easy">Level of Difficulty</option>
        <option value="easy">Easy</option>
        <option value="medium">Medium</option>
        <option value="hard">Difficult</option>
      </select>
      <select bind:value={numQuestions}>
        <option value="5">Number of Questions</option>
        <option value="5">5</option>
        <option value="10">10</option>
        <option value="15">15</option>
        <option value="20">20</option>
      </select>
      <select bind:value={category}>
        <option value="9">Category</option>
        <option value="9">General Knowledge</option>
        <option value="12">Music</option>
        <option value="11">Film</option>
        <option value="14">Television</option>
        <option value="17">Science & Nature</option>
        <option value="18">Computers</option>
        <option value="19">Mathmatics</option>
        <option value="21">Sports</option>
        <option value="22">Geography</option>
        <option value="23">History</option>
        <option value="24">Politics</option>
        <option value="25">Art</option>
        <option value="26">Celebrities</option>
        <option value="27">Animals</option>
        <option value="28">Vehicles</option>
      </select>
      <button type="submit">Start Game</button>
    </form>
    {#if isLoading}
      <div>
        <Icon class="icon" data={spinner} spin />
      </div>
    {/if}
  {/if}
  {#if formSubmitted}
    <TriviaCard
      {score}
      {toggleFormSubmitted}
      {addScore}
      {toggleForm}
      {toggleLoader}
      {trivia}
      {answers} />
  {/if}
</main>
