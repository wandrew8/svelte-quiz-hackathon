<script>
  import { onMount } from "svelte";

  let trivia;

  const loadTrivia = async () => {
    const res = await fetch(
      "https://opentdb.com/api.php?amount=10&category=11&difficulty=easy&type=multiple"
    );
    const data = await res.json();
    console.log(data);
  };

  onMount(() => {
    trivie = loadTrivia;
  });
</script>

<div>
  <h1>Hello</h1>
</div>
