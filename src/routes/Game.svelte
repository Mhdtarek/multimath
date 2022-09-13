<script>
  // @ts-nocheck

  import {
    MaterialApp,
    Button,
    Row,
    Col,
    Container,
    Divider,
    TextField,
  } from "svelte-materialify";
  import { initializeApp } from "firebase/app";
  import { getFirestore } from "firebase/firestore";
  import { doc, onSnapshot, updateDoc, setDoc } from "firebase/firestore";
  export let params = {};
  let questions = [];
  let equations = [];
  let game = {};
  let players = [];
  let hasJoinedGame = false;
  let userArrayNumber = 0;
  let playersScore = [];
  let endScore = 0;

  const rules = [(v) => v.length <= 20 || "Max 20 characters"];
  $: name = "";

  const firebaseConfig = {
    apiKey: "AIzaSyD4Aq2zW_aVeeiWv1Argn3OhuGcjH0Heps",
    authDomain: "multimath-9eb3f.firebaseapp.com",
    projectId: "multimath-9eb3f",
    storageBucket: "multimath-9eb3f.appspot.com",
    messagingSenderId: "353241258367",
    appId: "1:353241258367:web:ddd6d966a5d0acaecb224a",
    measurementId: "G-BRWMTFYP0D",
  };

  function generateEquation() {
    var a = Math.floor(Math.random() * 20) + 1;
    var b = Math.floor(Math.random() * 20) + 1;
    var op = equations[Math.floor(Math.random() * equations.length)];
    return a + op + b;
  }

  function newQuestion() {
    var oneThree = getRandomInt(1, 3);
    questions = [
      generateEquation(),
      generateEquation(),
      generateEquation(),
      generateEquation(),
      0,
      oneThree,
    ];
    questions[4] = eval(questions[oneThree]);

    // console.log(questions)
  }

  // check if the answer is correct
  function ifCorrectAnswer(equation, currentQuestion) {
    //calculate the equation
    let checkIfCorrect = eval(equation);
    //console.log(equation, currentQuestion)
    if (checkIfCorrect != currentQuestion) {
      playersScore[userArrayNumber].score =
        playersScore[userArrayNumber].score - 10;

      newQuestion();
      updateScore();
      console.log("Fel");
      return;
    }
    newQuestion();
    playersScore = players;
    //add to players score array so that it can be added to the db
    playersScore[userArrayNumber].score =
      playersScore[userArrayNumber].score + 10;

    updateScore();
  }

  //get random int from min , max
  function getRandomInt(min, max) {
    min = Math.ceil(min);
    max = Math.floor(max);
    return Math.floor(Math.random() * (max - min + 1)) + min; //The maximum is inclusive and the minimum is inclusive
  }

  const app = initializeApp(firebaseConfig);
  const db = getFirestore(app);

  function readGame(gameId) {
    const unsub = onSnapshot(doc(db, "games", params.id), (doc) => {
      console.log("(GAME) Current data: ", doc.data());
      game = doc.data();
      players = doc.data().players;
      equations = doc.data().equations;
    });
  }

  async function joinGame() {
    // Add a new document in collection "cities"
    await updateDoc(doc(db, "games", params.id), {
      players: [...players, { name: name, score: 0 }],
      playerCount: game.playerCount++,
    });
    newQuestion();
    hasJoinedGame = true;
    userArrayNumber = players.length - 1;
  }

  async function updateScore() {
    await updateDoc(doc(db, "games", params.id), {
      players: playersScore,
    });
  }
  readGame();
  newQuestion();
</script>

<main>
  <MaterialApp>
    {#if game.gameState === "Starting"}
      {#if !hasJoinedGame}
        <Row>
          <Col />
          <Col>
            <TextField clearable counter={20} bind:value={name} {rules}>
              namn
            </TextField>
            <Button on:click={joinGame}>spela</Button>
          </Col>
          <Col />
        </Row>
      {/if}
      {#if hasJoinedGame}
        <p class="font-weight-black text-center ">VÄNTA</p>
        <p class="text-center">Du heter {name}</p>
      {/if}
    {/if}

    {#if game.gameState === "started"}
      {#if hasJoinedGame}
        <Container>
          <h4 class="text-h3 text-center">{questions[4]}</h4>
          <Divider style="margin-bottom:20 0;" inset />
          <Row>
            <Col class="text-center">
              <Button
                size="x-large"
                on:click={() => ifCorrectAnswer(questions[0], questions[4])}
                class="indigo lighten-2"
                block>{questions[0]}</Button
              >
            </Col>
            <Col class="text-center">
              <Button
                size="x-large"
                on:click={() => ifCorrectAnswer(questions[1], questions[4])}
                class="indigo lighten-1"
                block>{questions[1]}</Button
              >
            </Col>
          </Row>
          <Row>
            <Col class="text-center">
              <Button
                size="x-large"
                on:click={() => ifCorrectAnswer(questions[2], questions[4])}
                class="indigo lighten-3"
                block>{questions[2]}</Button
              >
            </Col>
            <Col class="text-center">
              <Button
                size="x-large"
                on:click={() => ifCorrectAnswer(questions[3], questions[4])}
                class="indigo lighten-4"
                block>{questions[3]}</Button
              >
            </Col>
          </Row>
        </Container>
      {/if}
      {#if !hasJoinedGame}
        <p>Spelet har redan Startat</p>
      {/if}
    {/if}

    {#if game.gameState === "ended"}
      <p class="text-center">Spelet har slutat! Se på skärmen din resultat.</p>
    {/if}
  </MaterialApp>
</main>
