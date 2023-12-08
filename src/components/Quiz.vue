<template>
  <div class="container mx-auto p-4">
    <h1 class="text-2xl font-bold mb-4">Quiz App</h1>
    <div v-if="!quizFinished" class="bg-white p-6 rounded shadow-md">
      <h2 class="text-xl font-semibold mb-3">{{ currentQuestion.question }}</h2>
      <ul>
        <li v-for="(option, index) in currentQuestion.options" :key="index" :class="optionClass(option)" class="mb-2">
          <button class="w-full text-left p-2 rounded" @click="selectOption(option)">
            {{ option }}
          </button>
        </li>
      </ul>
      <button class="mt-4 px-4 py-2 bg-blue-500 text-white rounded" @click="nextQuestion" :disabled="!selectedAnswer">
        Next
      </button>
    </div>

    <div v-else class="text-center">
      <h2 class="text-xl font-semibold mb-3">Quiz Finished!</h2>
      <button class="mt-4 px-4 py-2 bg-green-500 text-white rounded" @click="restartQuiz">Restart Quiz</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import questions from '../assets/questions.json';

const currentQuestionIndex = ref(0);
const currentQuestion = ref(questions[currentQuestionIndex.value]);
const selectedAnswer = ref(null);
const isAnswerCorrect = ref(false);
const quizFinished = ref(false);
const selectedCategory = ref(null);
const numberOfQuestions = ref(null);

const selectOption = (option) => {
  selectedAnswer.value = option;
  isAnswerCorrect.value = option === currentQuestion.value.answer;
};

const nextQuestion = () => {
  if (currentQuestionIndex.value < questions.length - 1) {
    currentQuestionIndex.value++;
    currentQuestion.value = questions[currentQuestionIndex.value];
    selectedAnswer.value = null;
    isAnswerCorrect.value = false;
  } else {
    quizFinished.value = true;
  }
};

const restartQuiz = () => {
  currentQuestionIndex.value = 0;
  currentQuestion.value = questions[0];
  selectedAnswer.value = null;
  isAnswerCorrect.value = false;
  quizFinished.value = false;
};

// Define the optionClass method
const optionClass = (option) => {
  if (selectedAnswer.value === option) {
    return isAnswerCorrect.value ? 'bg-green-200' : 'bg-red-200';
  }
  return 'bg-gray-200';
};
</script>
