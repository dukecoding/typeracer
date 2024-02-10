<script setup>
import { ref } from 'vue'

// Some text for testing
const text = 'Lorem ipsum dolor sit amet consectetur adipisicing elit.'

const inputValue = ref('')

const words = text.split(' ')

function clear() {
  inputValue.value = ''
}

function wordCheck() {
  // In the last word there's no need for space character
  let space = ' '
  if (words.length === 1) {
    space = ''
  }
  if (inputValue.value === words[0] + space) {
    clear()
    words.shift()
  }
}
</script>

<template>
  <main>
    <h1>Welcome to <span id="game-title">Type Racer</span> game 🏎️</h1>
    <input v-model="inputValue" @keyup="wordCheck()" type="text" />
    <p>
      <span v-for="word in words">{{ word + ' ' }}</span>
    </p>
  </main>
</template>

<style scoped>
h1 {
  text-align: center;
}
p {
  padding-top: 2em;
}
input {
  width: 100%;
  padding: 0.5rem 0.5rem;
  font-size: var(--input-f-size);
  border: 2px var(--dark-green) solid;
  border-radius: 3px;
  background-color: var(--input-bg-color);
  color: var(--normal-text);
}
.wrong {
  background-color: var(--dark-purple);
}
.correct {
  background-color: green;
}
#game-title {
  background: linear-gradient(45deg, var(--dark-green), var(--dark-purple));
  border-radius: 3px;
  padding-inline: 0.4rem;
}
</style>
