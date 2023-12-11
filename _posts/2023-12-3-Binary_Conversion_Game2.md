---
toc: false
layout: post
title: Binary Conversion Game V2
courses: { compsci: {week: 2 } } 
type: hacks
hide: true
---
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Binary Number Guessing Game</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            text-align: center;
            margin: 50px;
            background-color: #f7f7f7;
        }
        h1 {
            color: #3498db;
        }
        p {
            color: #555;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            background-color: #3498db;
            color: #fff;
            border: none;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        button:hover {
            background-color: #2980b9;
        }
        label {
            display: block;
            margin-top: 20px;
            color: #333;
        }
        input {
            padding: 8px;
            font-size: 16px;
        }
        #result {
            margin-top: 20px;
            font-size: 18px;
            color: #e74c3c;
        }
        #binaryDisplay {
            font-size: 24px;
            margin-bottom: 20px;
            color: #333;
        }
        #decimalRange {
            display: flex;
            justify-content: space-between;
            margin-top: 20px;
            color: #555;
        }
        .distanceBar {
            width: 100%;
            height: 20px;
            position: relative;
            margin-top: 20px;
            background-color: #ecf0f1;
        }
        .distanceFill {
            height: 100%;
            position: absolute;
        }
        .distanceText {
            position: absolute;
            bottom: -20px;
            left: 50%;
            transform: translateX(-50%);
            font-size: 12px;
            color: #333;
        }
        #actualDistanceBar,
        #guessedDistanceBar {
            margin-bottom: 10px;
        }
        #actualDistanceFill {
            background-color: #2ecc71;
        }
        #guessedDistanceFill {
            background-color: #e74c3c;
        }
        /* Message popup styles */
        #message-popup {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            background-color: #2ecc71;
            color: #fff;
            padding: 15px;
            border-radius: 10px;
            text-align: center;
            display: none;
            z-index: 1;
        }
        #instructions-button {
            margin-top: 20px;
            background-color: #3498db;
            color: #fff;
        }
        /* Instructions Modal */
        #instructions-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            justify-content: center;
            align-items: center;
            z-index: 2;
        }
        #instructions-content {
            background: #fff;
            padding: 20px;
            border-radius: 10px;
            text-align: left;
        }
        #close-button {
            margin-top: 10px;
            background-color: #e74c3c;
            color: #fff;
        }
    </style>
</head>

<body>
    <h1>Binary Number Guessing Game</h1>
    <p>Convert the following binary number to decimal and enter the decimal value:</p>
    <!-- Display the binary version on the screen -->
    <div id="binaryDisplay"></div>
    <!-- Provide an input field for the user's guess -->
    <label for="userGuess">Enter your guess in decimal:</label>
    <input type="text" id="userGuess">
    <button onclick="checkGuess()">Guess Now</button>
    <button onclick="reload()">Try Again</button>
    <!-- Display the result on the page -->
    <div id="result"></div>
    <!-- Message popup for correct/incorrect answers -->
    <div id="message-popup" class="hidden"></div>
    <button id="instructions-button" onclick="showInstructions()">Instructions</button>
    <!-- Instructions Modal -->
    <div id="instructions-modal">
        <div id="instructions-content">
            <h2>Instructions for this 2 Player Game:</h2>
            <p>Goal: Be the first player to move forward 10 spaces and cross the finish line. <br><br> Getting Started: Click the "Start Game" button to initiate the game, then select the car you want. Start Game when you're ready. <br><br> How to play: A target decimal number will be randomly generated for each turn. Convert this number to the appropriate binary number and submit your answer. If your binary guess is correct, your car moves forward by one space. If your guess is incorrect, you lose your turn and your opponent gets a chance to get the right answer.</p>
            <button id="close-button" onclick="closeInstructions()">Close</button>
        </div>
    </div>
    <!-- Display the decimal range -->
    <div id="decimalRange">
        <span>0</span>
        <span>255</span>
    </div>
    <!-- Display the distance bars -->
    <div class="distanceBar" id="actualDistanceBar">
        <div class="distanceFill" id="actualDistanceFill"></div>
        <span class="distanceText">Actual Value</span>
    </div>
    <div class="distanceBar" id="guessedDistanceBar">
        <div class="distanceFill" id="guessedDistanceFill"></div>
        <span class="distanceText">Guessed Value</span>
    </div>
    <script>
        function reload() {
            location.reload()
        }
        // Function to convert decimal to binary with leading zeroes
        function decimalToBinary(decimal) {
            // Use toString(2) to convert to binary and padStart to add leading zeroes
            return decimal.toString(2).padStart(8, '0');
        }
        // Generate a random decimal number between 0 and 255
        const correctDecimal = Math.floor(Math.random() * 256);
        // Convert the decimal number to binary
        const correctBinary = decimalToBinary(correctDecimal);
        // Display the binary version on the screen after the page has loaded
        document.getElementById('binaryDisplay').textContent = `Binary: ${correctBinary}`;
        // Function to check the user's input
        function checkGuess() {
            // Get the user's input
            const userDecimalGuess = parseInt(document.getElementById('userGuess').
