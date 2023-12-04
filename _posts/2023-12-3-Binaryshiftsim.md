---
toc: true
title: Binary Shift Simulator
layout: default
description: A Binary Shift simulator
courses: { compsci: {week: 2} }
type: hacks
---

# Binary Shift Simulator

This simulator helps in understanding binary shift operations (left and right) with an interactive interface.

## HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Binary Shift Simulator</title>
    <style>
        /* CSS styles are included here */
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

        #binaryOutput, #decimalOutput, #shiftExplanation {
            margin-top: 20px;
            font-size: 18px;
            color: #333;
        }

        /* Additional responsive design styles can be added here */
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
        <!-- Additional interactive elements can be added here -->
    </div>
    <script>
        // JavaScript functions are included here

        function shiftLeft() {
            let binary = document.getElementById('binaryInput').value;
            let shifted = binary.substring(1) + '0';
            updateOutput(shifted);
        }

        function shiftRight() {
            let binary = document.getElementById('binaryInput').value;
            let shifted = '0' + binary.substring(0, binary.length - 1);
            updateOutput(shifted);
        }

        function generateRandomBinary() {
            let randomBinary = '';
            for(let i = 0; i < 16; i++) {
                randomBinary += Math.round(Math.random()).toString();
            }
            document.getElementById('binaryInput').value = randomBinary;
            updateOutput(randomBinary);
        }

        function updateOutput(shiftedBinary) {
            document.getElementById('binaryOutput').innerText = `Shifted Binary: ${shiftedBinary}`;
            document.getElementById('decimalOutput').innerText = `Decimal: ${parseInt(shiftedBinary, 2)}`;
            // Add explanation and overflow/underflow checks
        }

        // Additional functions for interactive learning aids and challenge mode can be implemented here
    </script>
</body>
</html>
