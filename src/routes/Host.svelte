<script lang="ts">
import { collection, addDoc } from "firebase/firestore"; 
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";
import { MaterialApp, Button, Chip, Icon, Container, Row, Col} from 'svelte-materialify';
import { doc, onSnapshot, updateDoc } from "firebase/firestore";
import { mdiAccount } from '@mdi/js';
import QRCode from "../lib/QRJS.svelte"

const firebaseConfig = {
    apiKey: "AIzaSyD4Aq2zW_aVeeiWv1Argn3OhuGcjH0Heps",
    authDomain: "multimath-9eb3f.firebaseapp.com",
    projectId: "multimath-9eb3f",
    storageBucket: "multimath-9eb3f.appspot.com",
    messagingSenderId: "353241258367",
    appId: "1:353241258367:web:ddd6d966a5d0acaecb224a",
    measurementId: "G-BRWMTFYP0D"
  };
const app = initializeApp(firebaseConfig);
const db = getFirestore(app);
let game = {}
let players = []
let gameCreated = false
let gameId = ""
let uri = ''
let playersCount = 0
let currentGameState = ""



// Add a new game with a generated id.
async function generateGame() {
  const docRef = await addDoc(collection(db, "games"), {
    gameState: "Starting",
    type: "default",
    playerCount: 0,
    players: [],
    scoreMode: "default"
  });
  readGame(docRef.id)
  gameCreated = true
  gameId = docRef.id
  uri = 'https://multimath.vercel.app/#/Game/' + gameId
}

async function StartGame(gameRef) {
  const washingtonRef = doc(db, "games", gameId);
  await updateDoc(washingtonRef, {
    gameState: "started"
  });


}


// read the game the host has created
function readGame(gameRef) {   
  const unsub = onSnapshot(doc(db, "games", gameRef), (doc) => {
    game = doc.data()
    console.log("Current data: ", doc.data());
    players = doc.data().players 
    playersCount = players.length
    currentGameState =  doc.data().gameState 

})}
readGame("read")
</script>


<main>
  
  {#if !gameCreated}
  <Button on:click={generateGame}>skapa spel</Button>
  {/if}   
  
  <Container>
  {#if gameCreated}
    {#if currentGameState === "Starting"}
    <div class="right">
      <Button size="small" on:click={StartGame}>starta spel</Button>
    </div>

    <div class="center ">
      <QRCode codeValue={uri} squareSize=150/>
    </div>
    {#each players as player}
      <Chip class="ma-2 blue darken-4 blue-text darken-1">
        <Icon path={mdiAccount} />
        <span>{player.name}</span>
      </Chip>
    {/each}
  {/if}
  {#if currentGameState === "started"}
    <Row>
      <Col>
      {#each players as player}
      <p>{player.name} {player.score}</p>
      {/each}
      </Col>
      <Col></Col>
    </Row>
  {/if}
 {/if}
</Container>
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