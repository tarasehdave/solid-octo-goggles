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
        <h1>Welcome to the Binary Number Conversion Game!</h1>
    </header>
</body>
    <main>
        <p>Binary Place Values:
        <p>00000001 (2^0): Represents 2^0 = 1
        <p>00000010 (2^1): Represents 2^1 = 2
        <p>00000100 (2^2): Represents 2^2 = 4
        <p>00001000 (2^3): Represents 2^3 = 8
        <p>00010000 (2^4): Represents 2^4 = 16
        <p> etc.
<body>
    <main>
        <p>Convert the binary number to decimal:</p>
        <button onclick="checkGuess()">Test Your Knowledge</button>
    </main>
</body>

{% assign BITS = 8 %}

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