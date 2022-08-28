<script>
// @ts-nocheck

  import { MaterialApp, Button, Row, Col, Container, Divider  } from 'svelte-materialify';
  import { initializeApp } from "firebase/app";
  import { getFirestore } from "firebase/firestore";
  import { doc, onSnapshot } from "firebase/firestore";
  let yes = "hi";
  let questions = []
  
  const firebaseConfig = {
    apiKey: "AIzaSyD4Aq2zW_aVeeiWv1Argn3OhuGcjH0Heps",
    authDomain: "multimath-9eb3f.firebaseapp.com",
    projectId: "multimath-9eb3f",
    storageBucket: "multimath-9eb3f.appspot.com",
    messagingSenderId: "353241258367",
    appId: "1:353241258367:web:ddd6d966a5d0acaecb224a",
    measurementId: "G-BRWMTFYP0D"
  };

  newQuestion()
  function generateEquation() {
    var a = Math.floor(Math.random() * 20) + 1;
    var b = Math.floor(Math.random() * 20) + 1;
    var op = ["*", "+", "/", "-"][Math.floor(Math.random()*4)];
    return  a + op + b ;
  }


  function newQuestion() {
    var oneThree = getRandomInt(1,3);
    questions = [generateEquation(), generateEquation(), generateEquation(), generateEquation(), 0 , oneThree]
    questions[4] = eval(questions[oneThree])
    
    // console.log(questions)
  }

// check if the answer is correct
  function ifCorrectAnswer (equation, currentQuestion) {
    //calculate the equation
    let checkIfCorrect = eval(equation)
    //console.log(equation, currentQuestion)
    if (checkIfCorrect != currentQuestion) {
      console.log("whoops! not true!")
      newQuestion()
      return
    }
    newQuestion()
    console.log("correct")

  }

//get random int from min , max 
  function getRandomInt(min, max) {
    min = Math.ceil(min);
    max = Math.floor(max);
    return Math.floor(Math.random() * (max - min + 1)) + min; //The maximum is inclusive and the minimum is inclusive 
  }


const app = initializeApp(firebaseConfig);
const db = getFirestore(app);



/*
const unsub = onSnapshot(doc(db, "games", "MC3ZFFcrk10I14ze8bt3"), (doc) => {
    console.log("Current data: ", doc.data());
});
*/
</script>

<main>
  <MaterialApp>
    <Container>
      <h4 class="text-h3 text-center">{questions[4]}</h4>
      <Divider style="margin-bottom:20 0;" inset />
      <Row>
        <Col class="text-center">
            <Button size="x-large" on:click={() => ifCorrectAnswer(questions[0], questions[4])} class="indigo lighten-2" block>{questions[0]}</Button>
        </Col>
        <Col class="text-center">
          <Button size="x-large" on:click={() => ifCorrectAnswer(questions[1], questions[4])} class="indigo lighten-1" block>{questions[1]}</Button>
        </Col>
      </Row>
      <Row>
        <Col class="text-center">
          <Button size="x-large" on:click={() => ifCorrectAnswer(questions[2], questions[4])} class="indigo lighten-3" block>{questions[2]}</Button>
        </Col>
        <Col class="text-center">
          <Button size="x-large" on:click={() => ifCorrectAnswer(questions[3], questions[4])} class="indigo lighten-4" block>{questions[3]}</Button>
        </Col>
      </Row>
    </Container>
  </MaterialApp>
</main>