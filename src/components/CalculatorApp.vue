<template>
  <div class="calculator-app">
    <div class="display">{{ currentInput || previousInput || "0" }}</div>
    <div class="buttons">
      <button @click="clear">AC</button>
      <button @click="toggleSign">±</button>
      <button @click="percentage">%</button>
      <button @click="appendOperator('/')">÷</button>

      <button @click="appendDigit(7)">7</button>
      <button @click="appendDigit(8)">8</button>
      <button @click="appendDigit(9)">9</button>
      <button @click="appendOperator('*')">×</button>

      <button @click="appendDigit(4)">4</button>
      <button @click="appendDigit(5)">5</button>
      <button @click="appendDigit(6)">6</button>
      <button @click="appendOperator('-')">−</button>

      <button @click="appendDigit(1)">1</button>
      <button @click="appendDigit(2)">2</button>
      <button @click="appendDigit(3)">3</button>
      <button @click="appendOperator('+')">+</button>

      <button @click="appendDigit(0)" class="zero">0</button>
      <button @click="appendDot">.</button>
      <button @click="calculateResult">=</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const currentInput = ref("");
const previousInput = ref("");
const operator = ref(null);
const shouldClear = ref(false);

function appendDigit(digit) {
  console.log("appendDigit called with:", digit);
  if (shouldClear.value) {
    currentInput.value = "";
    shouldClear.value = false;
  }
  currentInput.value += digit;
  console.log("currentInput is now:", currentInput.value);
}

function appendOperator(op) {
  if (currentInput.value) {
    previousInput.value = currentInput.value;
    operator.value = op;
    shouldClear.value = true;
  }
}

function clear() {
  currentInput.value = "";
  previousInput.value = "";
  operator.value = null;
  shouldClear.value = false;
}

function toggleSign() {
  if (currentInput.value) {
    currentInput.value =
      currentInput.value.charAt(0) === "-"
        ? currentInput.value.slice(1)
        : `-${currentInput.value}`;
  }
}

function percentage() {
  if (currentInput.value) {
    currentInput.value = (parseFloat(currentInput.value) / 100).toString();
  }
}

function appendDot() {
  if (!currentInput.value.includes(".")) {
    currentInput.value += ".";
  }
}

function calculateResult() {
  if (previousInput.value && currentInput.value && operator.value) {
    const result = eval(
      `${previousInput.value} ${operator.value} ${currentInput.value}`
    );
    currentInput.value = result.toString();
    previousInput.value = "";
    operator.value = null;
    shouldClear.value = true;
  }
}
</script>

<style scoped>
.calculator-app {
  width: 320px;
  height: 480px;
  background-color: #f2f2f2;
  border-radius: 20px;
  padding: 20px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
}

.display {
  width: 100%;
  height: 80px;
  background-color: #333;
  color: white;
  font-size: 50px;
  text-align: right;
  line-height: 80px;
  border-radius: 10px;
  padding: 10px;
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
  margin-top: 20px;
}

button {
  background-color: #f0f0f0;
  border: none;
  font-size: 24px;
  padding: 20px;
  border-radius: 10px;
  cursor: pointer;
  transition: background-color 0.2s;
}

button:active {
  background-color: #e0e0e0;
}

button.zero {
  grid-column: span 2;
}
</style>
