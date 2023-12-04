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
            font-family: Times, serif;
            text-align: center;
            margin: 50px;
        }
    </style>
    <!-- Move the script tag to the head section -->
    <script>
        // Function to generate a random decimal number between 0 and 255
        function generateRandomDecimal() {
            return Math.floor(Math.random() * 256);
        }
        // Function to convert decimal to binary with leading zeroes
        function decimalToBinary(decimal) {
            return decimal.toString(2).padStart(8, '0');
        }
        // Function to check the user's input
        function checkGuess() {
            const correctDecimal = generateRandomDecimal();
            const correctBinary = decimalToBinary(correctDecimal);
            // Get the user's input with error handling
            let userDecimalGuess;
            do {
                userDecimalGuess = prompt(`Convert ${correctBinary} to decimal and enter the decimal value:`);
            } while (!isValidDecimal(userDecimalGuess));
            // Check if the guess is correct
            userDecimalGuess = parseInt(userDecimalGuess);
            if (userDecimalGuess === correctDecimal) {
                alert('Well done! The decimal value you entered was correct!');
            } else {
                alert(`Sorry, the correct decimal value was ${correctDecimal}. Try again!`);
            }
        }
        // Function to validate if the input is a valid decimal
        function isValidDecimal(value) {
            return !isNaN(value) && value !== '';
        }
    </script>
</head>
<body>
    <header>
        <h1>Welcome to the Binary Number Guessing Game</h1>
    </header>
    <main>
        <p>Convert the following binary number to decimal and enter the decimal value:</p>
        <button onclick="checkGuess()">Test Your Knowledge</button>
    </main>
</body>
</html>
