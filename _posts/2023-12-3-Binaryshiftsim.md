---
toc: true
title: Binary Shift Simulator
layout: default
description: A Binary Shift simulator
courses: { compsci: {week: 2} }
type: hacks
---

# Binary Shift Simulator

This simulator helps in understanding binary shift operations (left and right) with an interactive interface and provides feedback to enhance learning.

## HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Binary Shift Simulator</title>
    <style>
        /* CSS styles */
        body {
            font-family: Arial, sans-serif;
            text-align: center;
        }
        #app {
            margin: 0 auto;
            width: 80%;
            padding: 20px;
        }
        input, button {
            margin: 10px;
            padding: 10px;
            font-size: 16px;
        }
        #binaryOutput, #decimalOutput, #shiftExplanation, #feedback {
            margin-top: 20px;
            font-size: 18px;
            color: #333;
        }
        .error {
            color: red;
        }
        .success {
            color: green;
        }
        /* Responsive design styles */
    </style>
</head>
<body>
    <div id="app">
        <h1>Binary Shift Simulator</h1>
        <input type="text" id="binaryInput" placeholder="Enter Binary Number" maxlength="16">
        <button onclick="generateRandomBinary()">Generate Random</button>
        <div>
            <button onclick="shiftLeft()">Shift Left (<<)</button>
            <button onclick="shiftRight()">Shift Right (>>)</button>
        </div>
        <div id="binaryOutput"></div>
        <div id="decimalOutput"></div>
        <div id="shiftExplanation"></div>
        <div id="feedback"></div> <!-- Feedback display area -->
    </div>
    <script>
        // JavaScript functions

        function isValidBinary(binary) {
            return /^[01]+$/.test(binary);
        }

        function shiftLeft() {
            let binary = document.getElementById('binaryInput').value;
            if (isValidBinary(binary)) {
                let shifted = binary.substring(1) + '0';
                updateOutput(shifted);
                provideFeedback("Left shift operation performed successfully.");
            } else {
                provideFeedback("Invalid binary input. Please enter a binary number.", true);
            }
        }

        function shiftRight() {
            let binary = document.getElementById('binaryInput').value;
            if (isValidBinary(binary)) {
                let shifted = '0' + binary.substring(0, binary.length - 1);
                updateOutput(shifted);
                provideFeedback("Right shift operation performed successfully.");
            } else {
                provideFeedback("Invalid binary input. Please enter a binary number.", true);
            }
        }  

        function generateRandomBinary() {
            let randomBinary = '';
            for(let i = 0; i < 16; i++) {
                randomBinary += Math.round(Math.random()).toString();
            }
            document.getElementById('binaryInput').value = randomBinary;
            updateOutput(randomBinary);
            provideFeedback("Random binary number generated.");
        }

        function updateOutput(shiftedBinary) {
            document.getElementById('binaryOutput').innerText = `Shifted Binary: ${shiftedBinary}`;
            document.getElementById('decimalOutput').innerText = `Decimal: ${parseInt(shiftedBinary, 2)}`;
            // Add explanation and overflow/underflow checks
        }

        function provideFeedback(message, isError = false) {
            let feedbackElement = document.getElementById('feedback');
            feedbackElement.className = isError ? 'error' : 'success';
            feedbackElement.innerText = message;
        }

        // Additional functions for interactive learning aids and challenge mode
    </script>
</body>
</html>

