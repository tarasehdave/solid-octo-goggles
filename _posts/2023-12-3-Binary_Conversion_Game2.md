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
    <title>Welcome to the Binary Number Conversion Game!</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            text-align: center;
            margin: 50px;
            background-color: #f7f7f7;
        }
        /* Message popup styles */
        #message-popup {
            position: fixed;
            bottom : 20px;
            left: 50%;
            transform: translateX(-50%);
            background-color: #ff0000;
            color: #fff;
            padding: 15px;
            border-radius: 10px;
            text-align: center;
            display: none;
            }
        h1 {
            color: #333;
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
        #result {
            margin-top: 20px;
            font-size: 18px;
        }
        #binaryDisplay {
            font-size: 24px;
            margin-bottom: 20px;
        }
        #decimalRange {
            display: flex;
            justify-content: space-between;
            margin-top: 20px;
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
        #actualDistanceBar {
            margin-bottom: 10px;
        }
        #actualDistanceFill {
            background-color: #2ecc71;
        }
        #guessedDistanceFill {
            background-color: #e74c3c;
        }
    </style>
</head>
<body>
    <main>
    <div>Hints:
        <p>Binary Place Values:
        <p>00000001 (2^0): Represents 2^0 = 1
        <p>00000010 (2^1): Represents 2^1 = 2
        <p>00000100 (2^2): Represents 2^2 = 4
        <p>00001000 (2^3): Represents 2^3 = 8
        <p>00010000 (2^4): Represents 2^4 = 16
        <p> etc.
{% assign BITS = 8 %}
<header> Here is a visual representation! <header>
<style>
    td {
        text-align: center;
        vertical-align: middle;
    }
</style>

<table>
    <thead>
        <tr class="header" id="table">
            <th>Plus</th>
            <th>Binary</th>
            <th>Hexadecimal</th>
            <th>Decimal</th>
            <th>Minus</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><div class="button" id="add1" onclick="add(1)">+1</div></td>
            <td id="binary">00000000</td>
            <td id="hexadecimal">0</td>
            <td id="decimal">0</td>
            <td><div class="button" id="sub1" onclick="add(-1)">-1</div></td>
        </tr>
    </tbody>
</table>

{% comment %}
Liquid for loop includes last number, thus the Minus
{% endcomment %}
{% assign bits = BITS | minus: 1 %} 

<table>
    <thead>
        <tr>
            {% comment %}
            Build many bits
            {% endcomment %}
            {% for i in (0..bits) %}
            <th><img id="bulb{{ i }}" src="{{site.baseurl}}/images/off.png" alt="" width="40" height="Auto">
                <div class="button" id="butt{{ i }}" onclick="javascript:toggleBit({{ i }})">Turn on</div>
            </th>
            {% endfor %}
        </tr>
    </thead>
    <tbody>
        <tr>
            {% comment %}
            Value of bit
            {% endcomment %}
            {% for i in (0..bits) %}
            <td><input type='text' id="digit{{ i }}" Value="0" size="1" readonly></td>
            {% endfor %}
        </tr>
    </tbody>
</table>

<script>
    const BITS = {{ BITS }};
    const MAX = 2 ** BITS - 1;
    const MSG_ON = "Turn on";
    const IMAGE_ON = "{{site.baseurl}}/images/on.png";
    const MSG_OFF = "Turn off";
    const IMAGE_OFF = "{{site.baseurl}}/images/off.png"

    // return string with current value of each bit
    function getBits() {
        let bits = "";
        for(let i = 0; i < BITS; i++) {
            bits = bits + document.getElementById('digit' + i).value;
        }
        return bits;
    }
    // setter for Document Object Model (DOM) values
    function setConversions(binary) {
        document.getElementById('binary').innerHTML = binary;
        // Hexadecimal conversion
        document.getElementById('hexadecimal').innerHTML = parseInt(binary, 2).toString(16);
        // Decimal conversion
        document.getElementById('decimal').innerHTML = parseInt(binary, 2).toString();
    }
    // convert decimal to base 2 using modulo with divide method
    function decimal_2_base(decimal, base) {
        let conversion = "";
        // loop to convert to base
        do {
            let digit = decimal % base;           // obtain right most digit
            conversion = "" + digit + conversion; // what does this do? inserts digit to front of string
            decimal = ~~(decimal / base);         // what does this do? divides by base what is ~~? force whole number
        } while (decimal > 0);                    // why while at the end? 0 pads front of binary number
            // loop to pad with zeros
            if (base === 2) {                     // only pad for binary conversions
                for (let i = 0; conversion.length < BITS; i++) {
                    conversion = "0" + conversion;
            }
        }
        return conversion;
    }
    // toggle selected bit and recalculate
    function toggleBit(i) {
        //alert("Digit action: " + i );
        const dig = document.getElementById('digit' + i);
        const image = document.getElementById('bulb' + i);
        const butt = document.getElementById('butt' + i);
        // Change digit and visual
        if (image.src.match(IMAGE_ON)) {
            dig.value = 0;
            image.src = IMAGE_OFF;
            butt.innerHTML = MSG_ON;
        } else {
            dig.value = 1;
            image.src = IMAGE_ON;
            butt.innerHTML = MSG_OFF;
        }
        // Binary numbers
        const binary = getBits();
        setConversions(binary);
    }
    // add is positive integer, subtract is negative integer
    function add(n) {
        let binary = getBits();
        // convert to decimal and do math
        let decimal = parseInt(binary, 2);
        if (n > 0) {  // PLUS
            decimal = MAX === decimal ? 0 : decimal += n; // OVERFLOW or PLUS
        } else  {     // MINUS
            decimal = 0 === decimal ? MAX : decimal += n; // OVERFLOW or MINUS
        }
        // convert the result back to binary
        binary = decimal_2_base(decimal, 2);
        // update conversions
        setConversions(binary);
        // update bits
        for (let i = 0; i < binary.length; i++) {
            let digit = binary.substr(i, 1);
            document.getElementById('digit' + i).value = digit;
            if (digit === "1") {
                document.getElementById('bulb' + i).src = IMAGE_ON;
                document.getElementById('butt' + i).innerHTML = MSG_OFF;
            } else {
                document.getElementById('bulb' + i).src = IMAGE_OFF;
                document.getElementById('butt' + i).innerHTML = MSG_ON;
            }
        }
    }
</script>
<p>Convert the following binary number to decimal and enter the decimal value:</p>

<!-- Display the binary version on the screen -->
<div id="binaryDisplay"></div>

<!-- Provide an input field for the user's guess -->
<label for="userGuess">Answer in decimal:</label>
<input type="text" id="userGuess">
<button onclick="checkGuess()">Enter</button>
<button onclick="reload()">Reset</button>

<!-- Display the result on the page -->
<div id="result"></div>

<!-- Message popup for correct/incorrect answers -->
  <div id="message-popup" class="hidden"></div>

  <button id="instructions-button" onclick="showInstructions()">Instructions</button>
  <!-- Instructions Modal -->
  <div id="instructions-modal">
      <div id="instructions-content">
          <h2>Instructions for this Game:</h2>
          <p>Correctly convert the provided binary number to decimal. <br><br> How to Start: Enter your decimal guess into the white box. Click the "Enter" button to check the answer.  <br><br> How to play: A target decimal number will be randomly generated for each turn. Convert this number to the appropriate binary number and submit your answer. If your binary guess is correct, the screen will give you a correct prompt. If it is incorrect, you will can click the "Reset" button to try again.</p>

  <button id="close-button" onclick="closeInstructions()">Close</button>
        </div>
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
        const userDecimalGuess = parseInt(document.getElementById('userGuess').value);

        // Check if the guess is correct
        const resultElement = document.getElementById('result');
        if (userDecimalGuess === correctDecimal) {
            resultElement.textContent = 'Congratulations! You guessed the correct decimal value.';
        } else {
            resultElement.textContent = `Sorry, the correct decimal value was ${correctDecimal}. Try again!`;
        }

        // Update the distance bars
        updateDistanceBar(userDecimalGuess);
    }

    // Function to show the instructions modal
    function showInstructions() {
        document.getElementById('instructions-modal').style.display = 'flex';
    }

    // Function to close the instructions modal
    function closeInstructions() {
        document.getElementById('instructions-modal').style.display = 'none';
    }
</script>

</body>