# <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="styles.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,500;0,700;1,700&display=swap" rel="stylesheet">
    <title>Calculadora</title>
</head>
<body>
    <div class="grid-container">
        <div class="output">
            <div data-previous-operand class="previous-operand">456 +</div>
                <div data-current-operand="" class="current-operand">170</div>
            </div>
            <button data-all-clear class="span-two">AC</button>
            <button data-delete>DEL</button>
            <button data-operator class="operator">÷</button>
            <button data-number>1</button>
            <button data-number>2</button>
            <button data-number>3</button>
            <button data-operator class="operator">x</button>
            <button  data-number>4</button>
            <button  data-number>5</button>
            <button data-number>6</button>
            <button class="operator">+</button>
            <button data-number>7</button>
            <button  data-number>8</button>
            <button  data-number>9</button>
            <button data-operator class="operator">-</button>
            <button data-number class="span-two">.</button>
            <button  data-number>0</button>
            <button data-equals class="operator">=</button>
        </div>
   
    <script src="calculadora.js">
    </script>
</body>
</html>











css


* {
    box-sizing: border-box;
    font-family: "Roboto", sans-serif;
    font-weight: bold;
    margin: 0;
    padding: 0;
  }
  
  body {
    background: linear-gradient(to right, #233329, #41b883);
  }
  
  .grid-container {
    display: grid;
    justify-content: center;
    align-content: center;
    min-height: 100vh;
    grid-template-columns: repeat(4, 100px);
    grid-template-rows: minmax(120px, auto) repeat(5, 100px);
  }
  
  .grid-container > button {
    cursor: pointer;
    font-size: 2rem;
    border: none;
    outline: none;
    background-color: #111;
    color: #eee;
  }
  
  .grid-container > button:hover {
    background-color: #eee;
    color: #111;
  }
  
  .grid-container > .operator {
    background: #41b88375;
  }
  
  .span-two {
    grid-column: span 2;
  }
  
  .grid-container > .output {
    grid-column: 1 / -1;
    background: #222;
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    justify-content: space-around;
    padding: 10px;
    word-wrap: break-word;
    word-break: break-all;
  }
  
  .grid-container > .output > .previous-operand {
    color: rgba(255, 255, 255, 0.75);
    font-size: 1.5rem;
  }
  
  .grid-container > .output > .current-operand {
    color: white;
    font-size: 2.5rem;
  }
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  java script!
  
  
  
  
  
  
  
  
  const numberButtons = document.querySelectorAll("[data-number]");
const operationButtons = document.querySelectorAll("[data-operator]");
const equalsButton = document.querySelector("[data-equals]");
const deleteButton = document.querySelector("[data-delete]");
const allClearButton = document.querySelector("[data-all-clear]");
const previousOperandTextElement = document.querySelector(
  "[data-previous-operand]"
);
const currentOperandTextElement = document.querySelector(
  "[data-current-operand]"
);

class Calculator {
  constructor(previousOperandTextElement, currentOperandTextElement) {
    this.previousOperandTextElement = previousOperandTextElement;
    this.currentOperandTextElement = currentOperandTextElement;
    this.clear();
  }

  formatDisplayNumber(number) {
    const stringNumber = number.toString();

    const integerDigits = parseFloat(stringNumber.split(".")[0]);
    const decimalDigits = stringNumber.split(".")[1];

    let integerDisplay;

    if (isNaN(integerDigits)) {
      integerDisplay = "";
    } else {
      integerDisplay = integerDigits.toLocaleString("en", {
        maximumFractionDigits: 0,
      });
    }

    if (decimalDigits != null) {
      return `${integerDisplay}.${decimalDigits}`;
    } else {
      return integerDisplay;
    }
  }

  delete() {
    this.currentOperand = this.currentOperand.toString().slice(0, -1);
  }

  calculate() {
    let result;

    const _previousOperand = parseFloat(this.previousOperand);
    const _currentOperand = parseFloat(this.currentOperand);

    if (isNaN(_previousOperand) || isNaN(_currentOperand)) return;

    switch (this.operation) {
      case "+":
        result = _previousOperand + _currentOperand;
        break;
      case "-":
        result = _previousOperand - _currentOperand;
        break;
      case "÷":
        result = _previousOperand / _currentOperand;
        break;
      case "*":
        result = _previousOperand * _currentOperand;
        break;
      default:
        return;
    }

    this.currentOperand = result;
    this.operation = undefined;
    this.previousOperand = "";
  }

  chooseOperation(operation) {
    if (this.currentOperand === "") return;

    if (this.previousOperand !== "") {
      this.calculate();
    }

    this.operation = operation;

    this.previousOperand = this.currentOperand;
    this.currentOperand = "";
  }

  appendNumber(number) {
    if (this.currentOperand.includes(".") && number === ".") return;

    this.currentOperand = `${this.currentOperand}${number.toString()}`;
  }

  clear() {
    this.currentOperand = "";
    this.previousOperand = "";
    this.operation = undefined;
  }

  updateDisplay() {
    this.previousOperandTextElement.innerText = `${this.formatDisplayNumber(
      this.previousOperand
    )} ${this.operation || ""}`;
    this.currentOperandTextElement.innerText = this.formatDisplayNumber(
      this.currentOperand
    );
  }
}

const calculator = new Calculator(
  previousOperandTextElement,
  currentOperandTextElement
);

for (const numberButton of numberButtons) {
  numberButton.addEventListener("click", () => {
    calculator.appendNumber(numberButton.innerText);
    calculator.updateDisplay();
  });
}

for (const operationButton of operationButtons) {
  operationButton.addEventListener("click", () => {
    calculator.chooseOperation(operationButton.innerText);
    calculator.updateDisplay();
  });
}

allClearButton.addEventListener("click", () => {
  calculator.clear();
  calculator.updateDisplay();
});

equalsButton.addEventListener("click", () => {
  calculator.calculate();
  calculator.updateDisplay();
});

deleteButton.addEventListener("click", () => {
  calculator.delete();
  calculator.updateDisplay();
});
  
  
  








