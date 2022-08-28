<script lang="ts">
import { collection, addDoc } from "firebase/firestore"; 
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";
import { MaterialApp, Button} from 'svelte-materialify';
import { doc, onSnapshot } from "firebase/firestore";
import QrCode from "svelte-qrcode"

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
let gameCreated = false
let gameId = ""
let webUrl = "https://  "

// Add a new document with a generated id.
async function generateGame() {
const docRef = await addDoc(collection(db, "games"), {
    gameState: "Starting",
    type: "default",
    playerCount: 0,
    players: {},
    scoreMode: "default"
});
console.log("created")
readGame(docRef.id)
gameCreated = true
gameId = docRef.id
}

function readGame(docReff) {
    
    const unsub = onSnapshot(doc(db, "games", docReff), (doc) => {
        game = doc.data()
        console.log("Current data: ", doc.data());


});
}
</script>


<main>
{#if !gameCreated}
<Button on:click={generateGame}>skapa spel</Button>
 {/if}   
 {#if gameCreated}
his
 {/if}
</main>