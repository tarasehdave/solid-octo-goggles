---
toc: true
title: Binary Conversion Game
layout: default
description: A Binary Math application that will be used for the binary conversion game
courses: { compsci: {week: 2} }
type: hacks
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Binary Number Guessing Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 50px;
        }
    </style>
</head>
<body>

<script>
function getRandomWord() {
    const words = ["peyton", "tara", "ameer", "developer", "coding", "binary"];
    const randomIndex = Math.floor(Math.random() * words.length);
    return words[randomIndex];
  }
  
  function wordToBinary(word) {
    return Array.from(word).map(char => char.charCodeAt(0).toString(2)).join(' ');
  }
  
  function playBinaryWordGuessingGame() {
    const secretWord = getRandomWord();
    const binaryRepresentation = wordToBinary(secretWord);
    const wordLength = secretWord.length;
    let attempts = 0;
  
    alert("Welcome to the Binary Word Guessing Game!");
    alert(`Try to guess the binary representation of the word: ${secretWord}`);
  
    function makeGuess() {
      const playerGuess = prompt("Enter your binary word guess:");
  
      if (playerGuess !== binaryRepresentation) {
        alert(`Incorrect guess. For tester purposes the correct binary representation is: ${binaryRepresentation}`);
        attempts++;
        makeGuess();
      } else {
        alert(`Congratulations! You guessed the correct binary representation "${binaryRepresentation}" of the word "${secretWord}" in ${attempts} attempts.`);
        askToPlayAgain();
      }
    }
  
    function askToPlayAgain() {
      const playAgain = confirm("Do You Want to Play Again?");
      if (playAgain) {
        playBinaryWordGuessingGame();
      } else {
        alert("Thanks for Playing! Goodbye.");
      }
    }
  
    makeGuess();
  } 
playBinaryWordGuessingGame();
</script>


</body>
</html>