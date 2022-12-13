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
  let userArrayNumber1 = 0;
  let userArrayNumber2 = 0;
  let userArrayNumber3 = 0;
  let playersScore = [];
  let endScore = 0;
  let scorearraynumber = getRandomInt(1, 3);
  console.log(params);
  let score1 = [];
  let score2 = [];
  let score3 = [];
  let modscore1 = [];
  let modscore2 = [];
  let modscore3 = [];

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
    // set mod score array to default score array
    modscore1 = score1;
    modscore2 = score2;
    modscore3 = score3;

    //calculate the equation
    let checkIfCorrect = eval(equation);
    //console.log(equation, currentQuestion)
    if (checkIfCorrect != currentQuestion) {
      if (scorearraynumber == 1) {
        modscore1[userArrayNumber1].score =
          modscore1[userArrayNumber1].score - 10;
      }
      if (scorearraynumber == 2) {
        modscore2[userArrayNumber2].score =
          modscore2[userArrayNumber2].score - 10;
      }
      if (scorearraynumber == 3) {
        modscore3[userArrayNumber3].score =
          modscore3[userArrayNumber3].score - 10;
      }

      newQuestion();
      updateScore();
      console.log("Fel");
      return;
    }
    newQuestion();
    //add to players score array so that it can be added to the db
    if (scorearraynumber == 1) {
      modscore1[userArrayNumber1].score =
        modscore1[userArrayNumber1].score + 10;
      updateScore();
    }
    if (scorearraynumber == 2) {
      modscore2[userArrayNumber2].score =
        modscore2[userArrayNumber2].score + 10;
      updateScore();
    }
    if (scorearraynumber == 3) {
      modscore3[userArrayNumber3].score =
        modscore3[userArrayNumber3].score + 10;
      updateScore();
    }
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

      score1 = doc.data().score1;
      score2 = doc.data().score2;
      score3 = doc.data().score3;

      console.log(
        "(GAME)",
        userArrayNumber1,
        userArrayNumber2,
        userArrayNumber3
      );
      console.log(
        "(GAME) score1: ",
        score1,
        "score2: ",
        score2,
        "score3: ",
        score3
      );
    });
  }

  async function joinGame() {
    if (scorearraynumber == 1) {
      await updateDoc(doc(db, "games", params.id), {
        score1: [
          ...score1,
          { name: name, score: 0, playescorenumber: scorearraynumber },
        ],
      });
    }
    if (scorearraynumber == 2) {
      await updateDoc(doc(db, "games", params.id), {
        score2: [
          ...score2,
          { name: name, score: 0, playescorenumber: scorearraynumber },
        ],
      });
    }
    if (scorearraynumber == 3) {
      await updateDoc(doc(db, "games", params.id), {
        score3: [
          ...score3,
          { name: name, score: 0, playescorenumber: scorearraynumber },
        ],
      });
    }
    newQuestion();
    hasJoinedGame = true;
    userArrayNumber1 = score1.length - 1;
    userArrayNumber2 = score2.length - 1;
    userArrayNumber3 = score3.length - 1;
  }

  async function updateScore() {
    if (scorearraynumber == 1) {
      await updateDoc(doc(db, "games", params.id), {
        score1: score1,
      });
    }
    if (scorearraynumber == 2) {
      await updateDoc(doc(db, "games", params.id), {
        score2: score2,
      });
    }
    if (scorearraynumber == 3) {
      await updateDoc(doc(db, "games", params.id), {
        score3: score3,
      });
    }
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
