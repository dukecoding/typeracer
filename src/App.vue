<script setup>
import { computed, ref } from 'vue'

const timer = ref('00:00.00')

// Some text for testing
const text = 'Lorem ipsum dolor.'
const words = text.split(' ')
let wordsDone = 0

const wordsDoneToCountPercentage = ref(0)

const progressPercentage = computed(() => {
  return Math.floor((wordsDoneToCountPercentage.value / words.length) * 100)
})

const inputValue = ref('')

function clear() {
  inputValue.value = ''
}

function wordCheck() {
  // In the last word there's no need for space character
  let space = ' '
  if (words.length - 1 === wordsDone) {
    space = ''
  }
  if (inputValue.value === words[wordsDone] + space) {
    wordsDone++
    wordsDoneToCountPercentage.value += 1
    clear()
  }
}

function start() {
  let miliseconds = 0
  let seconds = 0
  let minutes = 0
  let milisecondsZero = ''
  let secondsZero = ''
  let minutesZero = ''

  setInterval(() => {
    miliseconds++
    if (miliseconds === 100) {
      seconds++
      miliseconds = 0
      if (seconds === 60) {
        minutes++
        seconds = 0
      }
    }

    miliseconds < 10 ? (milisecondsZero = '0') : (milisecondsZero = '')
    seconds < 10 ? (secondsZero = '0') : (secondsZero = '')
    minutes < 10 ? (minutesZero = '0') : (minutesZero = '')

    timer.value = `${minutesZero}${minutes}:${secondsZero}${seconds}.${milisecondsZero}${miliseconds}`
  }, 10)
}
</script>

<template>
  <main>
    <h1>Welcome to <span id="game-title">Type Racer</span> game 🏎️</h1>
    <section class="flex">
      <input
        v-model="inputValue"
        @focus="start()"
        @keyup="wordCheck()"
        type="text"
        placeholder="Type here"
      />
      <section id="stats-container">
        <span id="timer">{{ timer }}</span>
        <span v-if="progressPercentage > 60" id="progressLight"
          >{{ progressPercentage }}%</span
        >
        <span v-else id="progressDark">{{ progressPercentage }}%</span>
      </section>
    </section>
    <section id="preview-container">
      <p>
        <span v-for="(word, idx) in words">
          <span v-if="idx < wordsDone" class="done">{{ word + ' ' }}</span>
          <span v-else>{{ word + ' ' }}</span>
        </span>
      </p>
    </section>
  </main>
</template>

<style scoped>
h1 {
  text-align: center;
}
input {
  width: 60%;
  padding: 0.5rem 0.5rem;
  font-size: var(--input-f-size);
  border: 2px var(--dark-green) solid;
  border-radius: 3px;
  background-color: var(--input-bg-color);
  color: var(--normal-text);
}
.done {
  background-color: var(--dark-green);
}
.flex {
  display: flex;
  justify-content: space-between;
}
#game-title {
  background: linear-gradient(45deg, var(--dark-green), var(--dark-purple));
  border-radius: 3px;
  padding-inline: 0.4rem;
}
#preview-container {
  padding-top: 2em;
}
#stats-container {
  width: 30%;
  padding: 0.5rem 0.5rem;
  font-size: var(--input-f-size);
  border: 2px var(--dark-green) solid;
  border-radius: 3px;
  background-color: var(--input-bg-color);
  color: var(--normal-text);
  display: flex;
  justify-content: space-between;
}
#progressLight {
  color: greenyellow;
}
#progressDark {
  color: green;
}
</style>
