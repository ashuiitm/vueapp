<template>
  <div class="mcq-container">
    <h3>{{ currentQuestion.text }}</h3>
    <ul>
      <li
        v-for="(option, index) in currentQuestion.options"
        :key="index"
        :class="{ 
          selected: selectedAnswer === option, 
          correct: isCorrect(option), 
          wrong: isWrong(option) 
        }"
        @click="selectOption(option)"
      >
        {{ option }}
      </li>
    </ul>

    <div v-if="selectedAnswer">
      <p v-if="selectedAnswer === currentQuestion.correct" style="color: green;">
        ✅ Correct!
      </p>
      <p v-else style="color: red;">
        ❌ Incorrect. Correct answer: {{ currentQuestion.correct }}
      </p>

      <button v-if="hasNextQuestion" @click="nextQuestion" style="margin-top: 15px;">
        Next Question ➡️
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const questions = [
  {
    text: 'Which language is used to build Vue.js apps?',
    options: ['Python', 'Java', 'JavaScript', 'C++'],
    correct: 'JavaScript'
  },
  {
    text: 'Which language is mostly used for Machine Learning?',
    options: ['Java', 'Python', 'Ruby', 'Go'],
    correct: 'Python'
  }
];

const currentIndex = ref(0);
const selectedAnswer = ref('');

const currentQuestion = computed(() => questions[currentIndex.value]);
const hasNextQuestion = computed(() => currentIndex.value < questions.length - 1);

function selectOption(option) {
  if (!selectedAnswer.value) {
    selectedAnswer.value = option;
  }
}

function nextQuestion() {
  if (hasNextQuestion.value) {
    currentIndex.value += 1;
    selectedAnswer.value = '';
  }
}

function isCorrect(option) {
  return selectedAnswer.value && option === currentQuestion.value.correct;
}

function isWrong(option) {
  return (
    selectedAnswer.value &&
    option === selectedAnswer.value &&
    option !== currentQuestion.value.correct
  );
}
</script>

<style scoped>
.mcq-container {
  max-width: 500px;
  margin: 40px auto;
  border: 1px solid #ccc;
  padding: 20px;
  border-radius: 12px;
  font-family: sans-serif;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  margin: 8px 0;
  padding: 10px;
  background: #f2f2f2;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s;
}
li:hover {
  background-color: #e0e0e0;
}
li.selected {
  font-weight: bold;
}
li.correct {
  background-color: #c8f7c5;
}
li.wrong {
  background-color: #f8d7da;
}
button {
  padding: 8px 16px;
  font-size: 16px;
  cursor: pointer;
  background-color: #2c3e50;
  color: white;
  border: none;
  border-radius: 6px;
}
</style>
