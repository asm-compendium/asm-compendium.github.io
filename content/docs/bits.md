---
title: Bits
type: docs
prev: barrel-shifter/
# next: docs/folder/
---
# Calculate Bits Used in Unsigned Integer

Enter an unsigned integer to find out how many bits are used to represent it.

<label for="numberInput">Enter an unsigned integer:</label>
<input type="number" id="numberInput" min="0" step="1">
<button onclick="getNumberOfBits()">Calculate</button>
<p id="result"></p>

<script>
    function getNumberOfBits() {
        // Get the input value
        const number = parseInt(document.getElementById("numberInput").value);

        // Check if the input is a valid number
        if (isNaN(number) || number < 0) {
            alert("Please enter a valid unsigned integer.");
            return;
        }

        // Calculate the number of bits used to represent the number
        const bits = number.toString(2).length;

        // Display the result
        document.getElementById("result").innerText = `Number of bits used: ${bits}`;
    }
</script>