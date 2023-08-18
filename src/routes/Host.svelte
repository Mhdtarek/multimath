<script lang="ts">
  import { collection, addDoc } from "firebase/firestore";
  import { initializeApp } from "firebase/app";
  import { getFirestore } from "firebase/firestore";
  import {
    MaterialApp,
    Button,
    Chip,
    Icon,
    Container,
    Row,
    Col,
    TextField,
    Select,
  } from "svelte-materialify";
  import { doc, onSnapshot, updateDoc } from "firebase/firestore";
  import QRCode from "../lib/QRJS.svelte";

  const firebaseConfig = {
    apiKey: "AIzaSyD4Aq2zW_aVeeiWv1Argn3OhuGcjH0Heps",
    authDomain: "multimath-9eb3f.firebaseapp.com",
    projectId: "multimath-9eb3f",
    storageBucket: "multimath-9eb3f.appspot.com",
    messagingSenderId: "353241258367",
    appId: "1:353241258367:web:ddd6d966a5d0acaecb224a",
    measurementId: "G-BRWMTFYP0D",
  };
  const app = initializeApp(firebaseConfig);
  const db = getFirestore(app);
  let game = {};
  let players = [];
  let gameCreated = false;
  let gameId = "";
  let uri = "";
  let playersCount = 0;
  let currentGameState = "";
  let maxScore = "";
  let maxScoreNumber = 100;
  let playerscheck = [];
  let value = [];
  let scores = [];
  const items = [
    { name: "+, -", value: ["+", "-"] },
    { name: "*, /", value: ["*", "-"] },
    { name: "*, /, +, -", value: ["*", "-", "+", "/"] },
    { name: "*, +", value: ["*", "+"] },
    { name: "/, -", value: ["/", "-"] },
  ];
  const rules = [(v) => v.length <= 20 || "Max 20 characters"];

  // Add a new game with a generated id.
  async function generateGame() {
    const equations = value;
    const docRef = await addDoc(collection(db, "games"), {
      gameState: "Starting",
      type: "default",
      playerCount: 0,
      score1: [],
      score2: [],
      score3: [],
      scoreMode: "default",
      equations,
    });
    readGame(docRef.id);
    gameCreated = true;
    gameId = docRef.id;
    uri = `https://multimath.vercel.app/#/Game/${gameId}`;
    maxScoreNumber = Number(maxScore);
    console.log(maxScoreNumber);
  }

  async function StartGame(gameRef) {
    const washingtonRef = doc(db, "games", gameId);
    await updateDoc(washingtonRef, {
      gameState: "started",
    });
  }

  // read the game the host has created
  function readGame(gameRef) {
    const unsub = onSnapshot(doc(db, "games", gameRef), (doc) => {
      game = doc.data();
      console.log("Current data: ", doc.data());
      playersCount = players.length;
      console.log(players);
      currentGameState = doc.data().gameState;
      players = [
        ...doc.data().score1,
        ...doc.data().score2,
        ...doc.data().score3,
      ];
      console.log("Current scores:", scores);
      // Sort the players.
      players.sort(function (a, b) {
        return b.score - a.score;
      });

      //check if any user has the end score
      playerscheck = players.filter((player) => {
        return player.score === maxScoreNumber;
      });

      //if there is, end the game.
      if (playerscheck.length >= 1) {
        endGame();
      }
    });
  }
  async function endGame() {
    const washingtonRef = doc(db, "games", gameId);
    await updateDoc(washingtonRef, {
      gameState: "ended",
    });
  }
</script>

<main>
  <MaterialApp>
    {#if !gameCreated}
      <Row>
        <Col />
        <Col>
          <TextField clearable counter={20} bind:value={maxScore} {rules}>
            Poäng för att sluta spelet
          </TextField>
          <Select {items} bind:value>Typ av spel</Select>
          <Button on:click={generateGame}>skapa spel</Button>
        </Col>
        <Col />
      </Row>
    {/if}

    <Container>
      <!-- check if game created-->
      {#if gameCreated}
        <!-- check if game is starting-->
        {#if currentGameState === "Starting"}
          <div class="right">
            <Button size="small" on:click={StartGame}>starta spel</Button>
          </div>
          <div class="center ">
            <QRCode codeValue={uri} squareSize="150" />
          </div>
          {#each players as player}
            <Chip class="ma-2 blue darken-4 blue-text darken-1">
              <span>{player.name}</span>
            </Chip>
          {/each}
        {/if}
        <!-- check if game has started-->
        {#if currentGameState === "started"}
          <div
            class="elevation-2 yellow"
            style="padding: 30px; margin-bottom: 30px;"
          >
            <Row noGutters>
              <Col cols={4} offset={6} offset_md={4}>
                <div class="pa-2 text-center">
                  <h5>Flest poäng</h5>
                  <p>{players[0].name}</p>
                  <p><b>MÅL: få {maxScore} poäng</b></p>
                </div>
              </Col>
            </Row>
          </div>
          <div class="elevation-2">
            <Row noGutters>
              <Col cols={4} offset={6} offset_md={4}>
                <div class="pa-2 text-center">
                  {#each players as p}
                    <p>{p.name} | {p.score}</p>
                  {/each}
                </div>
              </Col>
            </Row>
          </div>
        {/if}
        {#if currentGameState === "ended"}
        {#if players[0]}
          <div class="elevation-2 green" style="padding: 30px; margin-bottom: 15px;">
            <Row noGutters>
              <Col cols={4} offset={6} offset_md={4}>
                <div class="darken-1 txt-center">
                  <h4>FÖRSTA PLATS</h4>
                  <p>{players[0].name} | {players[0].score}</p>
                </div>
              </Col>
            </Row>
          </div>
        {/if}
      
        {#if players[1]}
          <div class="elevation-2 yellow" style="padding: 30px; margin-bottom: 15px;">
            <Row noGutters>
              <Col cols={4} offset={6} offset_md={4}>
                <div class="darken-1 text-center">
                  <h4>ANDRA PLATS</h4>
                  <p>{players[1].name} | {players[1].score}</p>
                </div>
              </Col>
            </Row>
          </div>
        {/if}
      
        {#if players[2]}
          <div class="elevation-2 red" style="padding: 30px; margin-bottom: 15px;">
            <Row noGutters>
              <Col cols={4} offset={6} offset_md={4}>
                <div class="darken-1 text-center">
                  <h4>TREDJE PLATS</h4>
                  <p>{players[2].name} | {players[2].score}</p>
                </div>
              </Col>
            </Row>
          </div>
        {/if}
      
        {#if players.length > 3}
          <div class="elevation-2">
            <Row noGutters>
              <Col cols={4} offset={6} offset_md={4}>
                <div class="pa-2 text-center">
                  {#each players.slice(3) as p}
                    <p>{p.name} | {p.score}</p>
                  {/each}
                </div>
              </Col>
            </Row>
          </div>
        {/if}
      {/if}
          
      {/if}
    </Container>
  </MaterialApp>
</main>

<style>
  .center {
    display: flex;
    justify-content: center;
  }
  .right {
    display: flex;
    justify-content: right;
    margin-bottom: 0;
  }
</style>
