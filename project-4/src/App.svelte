<body>
  <div class="calculator">
    <div class="display" id="display">0</div>
    <div class="buttons">
      <button on:click={() => appendNumber('7')}>7</button>
      <button on:click={() => appendNumber('8')}>8</button>
      <button on:click={() => appendNumber('9')}>9</button>
      <button on:click={() => appendNumber('÷')}>÷</button>
      <button on:click={() => appendNumber('4')}>4</button>
      <button on:click={() => appendNumber('5')}>5</button>
      <button on:click={() => appendNumber('6')}>6</button>
      <button on:click={() => appendNumber('*')}>*</button>
      <button on:click={() => appendNumber('1')}>1</button>
      <button on:click={() => appendNumber('2')}>2</button>
      <button on:click={() => appendNumber('3')}>3</button>
      <button on:click={() => appendNumber('-')}>-</button>
      <button class="clear-button" on:click={clearDisplay}>AC</button>
      <button on:click={() => appendNumber('0')}>0</button>
      <button on:click={() => appendNumber('+')}>+</button>
      <button on:click={calculate}>=</button>
    </div>
  </div>
</body>

<style>
  body {
    background-color: #f1bb34; 
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
  }

  .calculator {
    background-color: rgb(19, 90, 96); 
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.2);
  }

  .buttons {
    margin-top: 10px;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-gap: 10px;
  }

  button {
    background-color: #f2f2f2; 
    border: none;
    padding: 15px;
    font-size: 18px;
    border-radius: 5px;
    cursor: pointer;
  }

  .clear-button {
    background-color: #f1bb34;
  }

  .display {
    grid-column: span 4;
    text-align: right;
    font-size: 24px;
    padding: 10px;
    background-color: #f2f2f2; 
    border-radius: 5px;
    margin-bottom: 10px;
  }
</style>

<script>
  let currentInput = '';
  let result = 0;

  function clearDisplay() {
    currentInput = '';
    result = 0;
    updateDisplay();
  }

  function appendNumber(number) {
    if (number === '=' && currentInput !== '') {
      result = eval(currentInput);
      currentInput = '';
    } else {
      currentInput += number;
    }
    updateDisplay();
  }

  function updateDisplay() {
    document.getElementById('display').innerText = result || currentInput || '0';
  }

  function calculate() {
    result = eval(currentInput);
    currentInput = '';
    updateDisplay();
  }
</script>