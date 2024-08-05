---
title: Division timing
type: docs
prev: barrel-shifter/
# next: docs/folder/
---

<style>
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type=number] {
  -moz-appearance: textfield;
}
</style>



### Division Cycle Count Calculator



<div style="display: grid; grid-template-columns: auto auto; gap: 10px; align-items: center; max-width: 300px;">
  <label for="numberInput">Dividend value (Rn):</label>
  <input type="number" id="numberInput" step="1" value="" style="width: 50%;" oninput="divCycles()">
  <label for="numberInput2">Divisor value (Rm):</label>
  <input type="number" id="numberInput2" step="1" value="" style="width: 50%;" oninput="divCycles()">
  <!-- <button style="grid-column: span 2; justify-self: left;" onclick="divCycles()">[ CALCULATE ]</button> -->
  <label for="result">Predicted cycles:</label>
  <input type="text" id="result" readonly  style="width: 50%;">
</div>

<script>
    function divCycles() {
        let number = Math.abs(parseInt(document.getElementById("numberInput").value));
        let number2 = Math.abs(parseInt(document.getElementById("numberInput2").value));

        if ( document.getElementById("numberInput").value === "" || document.getElementById("numberInput2").value === "" ) {
            document.getElementById("result").value = '';
            return;
        }
        if (isNaN(number) || number < 0 || number.toString(2).length > 32) {
            document.getElementById("result").value = 'Invalid';
            return;
        }

        if (isNaN(number2) || number2 < 0 || number2.toString(2).length > 32) {
            document.getElementById("result").value = 'Invalid';
            return;
        }

        const bits1 = number.toString(2).length;
        const bits2 = number2.toString(2).length;
        var res = 0;

        if (number === 0 || number2 === 0) {
            res = 2;
        } else if (bits1 < bits2){
            res = 3;
        } else {
            res = 5 + Math.floor((bits1 - bits2)/4)
        }

        
        document.getElementById("result").value = res;
    }
</script>