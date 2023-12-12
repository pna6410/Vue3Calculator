<script setup>
import { reactive, computed } from 'vue';

const data = reactive({
  number: 0,
  temp : 0,
  operator: null
})

function inputNumber(n) {
  if(data.number === 0) data.number = n
  else data.number = data.number * 10 + n
}

function inputOperator(value) {
  data.temp = data.number
  data.number = 0
  data.operator = value
}

const displayed = computed(() => {
  let text = ''
  if(data.temp) text += data.temp
  if(data.operator) text += data.operator
  if(data.number) text += data.number
  if(!text) text = 0
  return text
})

function calculate() {
  let result = eval(displayed.value)
  data.temp = 0
  data.operator = null
  data.number = result
}

function reset() {
  data.temp = 0
  data.operator = null
  data.number = 0
}

function getKey(event) {
  if(event.key.match(/[0-9]/)) {
    inputNumber(event.key * 1)
  } else if(['+', '-', '*', '/'].includes(event.key)) {
    inputOperator(event.key)
  } else if(event.key == '=' || event.key == 'Enter') {
    calculate()
  } else if(event.key == 'c') {
    reset()
  }
}

document.addEventListener('keyup', getKey)
</script>

<template>
  <div class="grid grid-cols-4 gap-4">

    <div class="col-span-4 text-center text-2xl text-white">Calculator</div>

    <div class="col-span-4 border p-4 text-right rounded-lg bg-white text-black">
      {{ displayed }}
    </div>

    <div class="grid grid-cols-3 gap-4 col-span-3">
      <button v-for="n of 9" @click="inputNumber(n)">{{ n }}</button>
      <button @click="inputNumber(0)">0</button>
      <button @click="reset">C</button>
      <button @click="calculate">=</button>
    </div>

    <div class="grid col-span-1 gap-4">
      <button v-for="o of ['+', '-', '*', '/']" @click="inputOperator(o)">{{ o }}</button>
    </div>
  </div>
</template>