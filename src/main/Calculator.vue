<template>
  <div className="calculator">
    <Display :value="displayValue" />

    <Button label="AC" @onClick="clearMemory" operation />
    <Button label="Del" @onClick="addDigit" operation />
    <Button label="%" @onClick="setOperation" operation />
    <Button label="/" @onClick="setOperation" operation />
    <Button label="7" @onClick="addDigit" />
    <Button label="9" @onClick="addDigit" />
    <Button label="8" @onClick="addDigit" />
    <Button label="*" @onClick="setOperation" operation />
    <Button label="4" @onClick="addDigit" />
    <Button label="5" @onClick="addDigit" />
    <Button label="6" @onClick="addDigit" />
    <Button label="-" @onClick="setOperation" operation />
    <Button label="1" @onClick="addDigit" />
    <Button label="2" @onClick="addDigit" />
    <Button label="3" @onClick="addDigit" />
    <Button label="+" @onClick="setOperation" operation />
    <Button label="0" @onClick="addDigit" />
    <Button label="+/-" @onClick="addDigit" />
    <Button label="." @onClick="addDigit" />
    <Button label="=" @onClick="setOperation" operation />
  </div>
</template>

<script>
import Display from "../components/Display";
import Button from "../components/Button";

export default {
  data: function() {
    return {
      displayValue: "0",
      clearDisplay: false,
      operation: null,
      values: [0, 0],
      current: 0
    };
  },
  components: { Button, Display },
  methods: {
    clearMemory() {
      Object.assign(this.$data, this.$options.data());
    },
    setOperation(operation) {
      if (this.current === 0) {
        this.operation = operation;
        this.current = 1;
        this.clearDisplay = true;
      } else {
        const equals = operation === "=";
        const currentOpeation = this.operation;
        const values = [...this.values];

        switch (currentOpeation) {
          case "+":
            values[0] = values[0] + values[1];
            break;
          case "-":
            values[0] = values[0] - values[1];
            break;
          case "*":
            values[0] = values[0] * values[1];
            break;
          case "/":
            values[0] = values[0] / values[1];
            break;
          case "%":
            values[0] = (values[0] / 100) * values[1];
            break;
          default:
            break;
        }

        this.displayValue = values[0]
        this.operation = equals ? null : operation
        this.current = equals ? 0 : 1
        this.clearDisplay = !equals
        this.values
      }
    },
    addDigit(n) {
      if (n === "." && this.displayValue.includes(".")) {
        return;
      }

      const clearDisplay = this.displayValue === "0" || this.clearDisplay;
      const currentValue = clearDisplay ? "" : this.displayValue;

      let displayValue = null;
      let uDigiro = currentValue.length;

      if (n === "Del") {
        if (uDigiro >= 1) {
          uDigiro = currentValue.length - 1;
          displayValue = currentValue.substring(0, uDigiro);
        } else if (uDigiro <= 0 || uDigiro === isNaN || uDigiro === undefined) {
          displayValue = "0";
        }
      } else if (n === "+/-") {
        displayValue = currentValue * -1;
      } else {
        displayValue = currentValue + n;
      }
      this.displayValue = displayValue;
      this.clearDisplay = false;

      if (n !== ".") {
        const i = this.current;
        const newValue = parseFloat(displayValue);
        this.values[i] = newValue;
      }
    }
  }
};
</script>

<style>
.calculator {
  height: 320px;
  width: 235px;
  border-radius: 5px;
  overflow: hidden;

  display: grid;
  grid-template-columns: repeat(4, 25%);
  grid-template-rows: 1fr 48px 48px 48px 48px 48px;
}
</style>
