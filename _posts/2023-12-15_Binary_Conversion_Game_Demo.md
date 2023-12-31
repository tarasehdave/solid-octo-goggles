---
toc: false
layout: post
title: Binary Conversion Game Demo
courses: { compsci: {week: 2 } } 
type: hacks
---
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Verdana, sans-serif;
            text-align: center;
            margin: 50px;
            background-image: url('./images/space.jpeg');
            background-repeat: no-repeat;
            background-size: cover;
            background-color: #410b73; /* Background color change */
        }
        /* Message popup styles */
        #message-popup {
            position: fixed;
            bottom: 20px;
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
            background-color: #db7a34;
            color: #fff;
            border: none;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        button:hover {
            background-color: #e88e4d;
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
    </style>
    <title>Welcome to the Binary Number Conversion Game!</title>
</head>
<body>
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
    <button id="instructions-button" onclick="showInstructions()">Open Information</button>
    <!-- Instructions Modal -->
    <div id="instructions-modal">
        <div id="instructions-content">
            <h2>Instructions:</h2>
            <p>Correctly convert the provided binary number to decimal. <br><br> How to Start: Enter your decimal guess into the white box. Click the "Enter" button to check the answer.  <br><br> How to play: A target decimal number will be randomly generated for each turn. Convert this number to the appropriate binary number and submit your answer. If your binary guess is correct, the screen will give you a correct prompt. If it is incorrect, you will can click the "Reset" button to try again.<br><br> Hints: <p>Binary Place Values:
            <p>00000001 (2^0): Represents 2^0 = 1
            <p>00000010 (2^1): Represents 2^1 = 2
            <p>00000100 (2^2): Represents 2^2 = 4
            <p>00001000 (2^3): Represents 2^3 = 8
            <p>00010000 (2^4): Represents 2^4 = 16
            <p> etc.</p>
        <button id="close-button" onclick="closeInstructions()">Close</button>
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
    </div>
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
        function decimal_