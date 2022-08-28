<script lang="ts">
import { collection, addDoc } from "firebase/firestore"; 
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";
import { MaterialApp, Button} from 'svelte-materialify';

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
  let docRefVar = ""
  var test = "yes"
// Add a new document with a generated id.
async function generateGame() {
const docRef = await addDoc(collection(db, "games"), {
    gameState: "Starting",
    type: "default",
    test
});
console.log("Document written with ID: ", docRef.id);
}

</script>


<main>

hi
<Button on:click={generateGame}></Button>
    
</main>