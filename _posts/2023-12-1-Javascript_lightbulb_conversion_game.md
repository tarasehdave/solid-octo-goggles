---
toc: true
title: Binary Lightbulb Conversion Game Improved
layout: default
description: A Binary Math application of lightbulbs that prompts the user to convert decimal and hexadecimal to binary
courses: { compsci: {week: 4} }
type: hacks
---

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

{% assign bits = BITS | minus: 1 %} 

<table>
    <thead>
        <tr>
            {% for i in (0..bits) %}
            <th><img id="bulb{{ i }}" src="{{site.baseurl}}/images/light-bulb-off.jpeg" alt="" width="40" height="Auto">
                <div class="button" id="butt{{ i }}" onclick="javascript:toggleBit({{ i }})">Turn on</div>
            </th>
            {% endfor %}
        </tr>
    </thead>
    <tbody>
        <tr>
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
    const IMAGE_ON = "{{site.baseurl}}/images/lightbulb_on.png";
    const MSG_OFF = "Turn off";
    const IMAGE_OFF = "{{site.baseurl}}/images/light-bulb-off.jpeg";

    function getRandomNumber() {
        return Math.floor(Math.random() * MAX) + 1;
    }

    function setRandomNumbers() {
        const randomDecimal = getRandomNumber();
        const randomHexadecimal = randomDecimal.toString(16).toUpperCase();
        const randomBinary = decimal_2_base(randomDecimal, 2);

        document.getElementById('binary').innerHTML = randomBinary;
        document.getElementById('hexadecimal').innerHTML = randomHexadecimal;
        document.getElementById('decimal').innerHTML = randomDecimal;

        for (let i = 0; i < BITS; i++) {
            document.getElementById('digit' + i).value = randomBinary[i];
            if (randomBinary[i] === "1") {
                document.getElementById('bulb' + i).src = IMAGE_ON;
                document.getElementById('butt' + i).innerHTML = MSG_OFF;
            } else {
                document.getElementById('bulb' + i).src = IMAGE_OFF;
                document.getElementById('butt' + i).innerHTML = MSG_ON;
            }
        }
    }

    function convertAndCheck() {
        const userBinary = getBits();
        const correctBinary = document.getElementById('binary').innerHTML;

        if (userBinary === correctBinary) {
            alert("Congratulations! That's correct.");
        } else {
            alert(`Sorry, the correct binary is ${correctBinary}.`);
        }

        setRandomNumbers();
    }

    function decimal_2_base(decimal, base) {
        // ... (unchanged)
    }

    function toggleBit(i) {
        // ... (unchanged)
    }

    function add(n) {
        // ... (unchanged)
    }

    // Initialize the game
    setRandomNumbers();
</script>
