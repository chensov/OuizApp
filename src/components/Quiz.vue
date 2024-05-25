<template>

  <div class="container mx-auto p-4 w-[50%] flex  justify-center h-screen flex-col z-[999]">

    <div v-if="!quizFinished" class=" p-6 bg-[#ECE2D9] m-6  rounded-3xl border border-[#847E78]/10  backdrop-filter shadow-2xl backdrop-blur bg-opacity-60 ">
      <h2 class="text-xl font-semibold mb-3 uppercase">{{ currentQuestion.question }}</h2>
      <ul>
        <li v-for="(option, index) in currentQuestion.options" :key="index" :class="optionClass(option)" class="mb-2">
          <button class="w-full text-left p-2 rounded" @click="selectOption(option)">
            {{ option }}
          </button>
        </li>
      </ul>
      <button class="mt-4 px-4 py-2 bg-[#A2DBF2] text-white rounded-3xl" @click="nextQuestion" :disabled="!selectedAnswer">
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
import questions from '../assets/capitalsQuestions.json';

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
    return isAnswerCorrect.value ? 'bg-[#90C4B6] rounded-xl text-slate-50' : 'bg-red-200';
  }
  return 'bg-slate-100 rounded-xl';
};
</script>
