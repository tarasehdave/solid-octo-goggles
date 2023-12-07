---
toc: true
title: Binary Conversion Game
layout: default
description: A Binary Math application that will be used for the binary conversion game
courses: { compsci: {week: 2} }
type: hacks
---

<html lang="en">
<style>
    header {
        background-color: #1d1c63;
        padding: 10px;
        border-radius: 10px;
    }
</style>
<style>
    body {
        background-color: #1d1c63;
        margin: 30px;
    }
</style>
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
    <!-- Move the script tag to the header -->
    <script>
        // Generate a random decimal number between 0 and 255
        function generateRandomDecimal() {
            return Math.floor(Math.random() * 256);
        }
        // Convert decimal to binary with leading zeroes
        function decimalToBinary(decimal) {
            return decimal.toString(2).padStart(8, '0');
        }
        // Check the user's input
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
        // Validate if the input is a valid decimal
        function isValidDecimal(value) {
            return !isNaN(value) && value !== '';
        }
    </script>
</head>
<body>
    <header>
        <h1>Welcome to the Binary Number Guessing Game!</h1>
    </header>
</body>
    <main>
        <p>Binary Place Values:
        <p>1 (2^0): Represents 2^0 = 1
        <p>10 (2^1): Represents 2^1 = 2
        <p>>100 (2^2): Represents 2^2 = 4
        <p>1000 (2^3): Represents 2^3 = 8
        <p>10000 (2^4): Represents 2^4 = 16
<body>
    <main>
        <p>Convert the binary number to decimal:</p>
        <button onclick="checkGuess()">Test Your Knowledge</button>
    </main>
</body>
</html>
