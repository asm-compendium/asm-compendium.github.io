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

<!-- <script id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>


$$ 
f(Rn, Rm) =
\begin{cases}
2, & \text{if } Rn = 0 \lor Rm = 0 \\\\
3, & \text{if } \text{bits}(Rn) < \text{bits}(Rm) \\\\
5 + \left\lfloor \frac{\text{bits}(Rn) - \text{bits}(Rm)}{4} \right\rfloor, & \text{otherwise} \\\\
\end{cases}
$$ -->

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
  <mi>f</mi>
  <mo stretchy="false">(</mo>
  <mi>R</mi>
  <mi>n</mi>
  <mo>,</mo>
  <mi>R</mi>
  <mi>m</mi>
  <mo stretchy="false">)</mo>
  <mo>=</mo>
  <mrow data-mjx-texclass="INNER">
    <mo data-mjx-texclass="OPEN">{</mo>
    <mtable columnalign="left left" columnspacing="1em" rowspacing=".2em">
      <mtr>
        <mtd>
          <mn>2</mn>
          <mo>,</mo>
        </mtd>
        <mtd>
          <mtext>if&#xA0;</mtext>
          <mi>R</mi>
          <mi>n</mi>
          <mo>=</mo>
          <mn>0</mn>
          <mo>&#x2228;</mo>
          <mi>R</mi>
          <mi>m</mi>
          <mo>=</mo>
          <mn>0</mn>
        </mtd>
      </mtr>
      <mtr>
        <mtd>
          <mn>3</mn>
          <mo>,</mo>
        </mtd>
        <mtd>
          <mtext>if&#xA0;</mtext>
          <mtext>bits</mtext>
          <mo stretchy="false">(</mo>
          <mi>R</mi>
          <mi>n</mi>
          <mo stretchy="false">)</mo>
          <mo>&lt;</mo>
          <mtext>bits</mtext>
          <mo stretchy="false">(</mo>
          <mi>R</mi>
          <mi>m</mi>
          <mo stretchy="false">)</mo>
        </mtd>
      </mtr>
      <mtr>
        <mtd>
          <mn>5</mn>
          <mo>+</mo>
          <mrow data-mjx-texclass="INNER">
            <mo data-mjx-texclass="OPEN">&#x230A;</mo>
            <mfrac>
              <mrow>
                <mtext>bits</mtext>
                <mo stretchy="false">(</mo>
                <mi>R</mi>
                <mi>n</mi>
                <mo stretchy="false">)</mo>
                <mo>&#x2212;</mo>
                <mtext>bits</mtext>
                <mo stretchy="false">(</mo>
                <mi>R</mi>
                <mi>m</mi>
                <mo stretchy="false">)</mo>
              </mrow>
              <mn>4</mn>
            </mfrac>
            <mo data-mjx-texclass="CLOSE">&#x230B;</mo>
          </mrow>
          <mo>,</mo>
        </mtd>
        <mtd>
          <mtext>otherwise</mtext>
        </mtd>
      </mtr>
    </mtable>
    <mo data-mjx-texclass="CLOSE" fence="true" stretchy="true" symmetric="true"></mo>
  </mrow>
</math>




### Division Cycle Count Calculator


<\br>
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
