<script>
  import { onMount } from "svelte";

  let trivia = [];
  let answers = [];

  const loadTrivia = async () => {};

  const sanitizeText = (text) => {
    const editedText = text.replace(/&#039;/g, "'");
    return editedText.replace(/(&quot\;)/g, '"');
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

  const getQuestions = (questions) => {
    const array = questions.forEach((question) => {
      let answersArray = question.incorrect_answers;
      answersArray.push(question.correct_answer);
      const randomizedArray = shuffle(answersArray);
      answers.push(randomizedArray);
    });
    return array;
  };

  const handleSubmit = (e, index) => {
    console.log(e.target.value);
    const userResponse = e.target.value;
    if (userResponse === trivia[index].correct_answer) {
      console.log("Correct");
    } else {
      console.log("Incorrect");
    }
    console.log(index);
  };

  onMount(async () => {
    const res = await fetch(
      "https://opentdb.com/api.php?amount=10&category=11&difficulty=easy&type=multiple"
    );
    const data = await res.json();
    trivia = data.results;
    getQuestions(trivia);
    console.log(trivia);
    console.log(answers);
  });
</script>

<style>
  .grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 2rem;
  }
</style>

<div>
  <div class="grid">

    {#each trivia as card, i}
      <div>
        <p>{sanitizeText(card.question)}</p>
        <form on:change={(e) => handleSubmit(e, i)}>
          <input
            type="radio"
            id={answers[i][0]}
            name="gender"
            value={answers[i][0]} />
          <label for={answers[i][0]}>{answers[i][0]}</label>
          <br />
          <input
            type="radio"
            id={answers[i][1]}
            name="gender"
            value={answers[i][1]} />
          <label for={answers[i][1]}>{answers[i][1]}</label>
          <br />
          <input
            type="radio"
            id={answers[i][2]}
            name="gender"
            value={answers[i][2]} />
          <label for={answers[i][2]}>{answers[i][2]}</label>
          <input
            type="radio"
            id={answers[i][3]}
            name="gender"
            value={answers[i][3]} />
          <label for={answers[i][3]}>{answers[i][3]}</label>
        </form>
      </div>
    {/each}
  </div>
</div>
