<template>
  <div class="d-flex justify-content-center">
    <div v-if="showScore" class="card" style="max-width: 20rem">
      <div class="card-body">
        <h5 class="card-title">
          Ton score est de {{ score }} sur {{ questions.length }}
        </h5>
        <button v-on:click="resetData" class="btn btn-outline-primary">
          Recommencer
        </button>
      </div>
    </div>
    <div v-else>
      <p>Question No.{{ currentQuestion + 1 }} of {{ questions.length }}</p>

      <div class="card">
        <div class="card-body">
          <div class="card-header text-center mb-3">
            <span class="fw-bold fs-3">{{ countDown }}</span>
          </div>
          <h5 class="card-title">
            {{ questions[currentQuestion].text }}
          </h5>

          <div class="d-flex flex-column gap-2 mt-4">
            <template
              v-for="(option, index) in questions[currentQuestion].options"
              :key="index"
            >
              <button
                class="btn btn-primary"
                v-on:click="handleAnswer(option.isCorrect)"
              >
                {{ option.text }}
              </button>
            </template>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import DATA from "../data";

const showScore = ref(false);
const score = ref(0);
const countDown = ref(30);
const timer = ref(null);
const currentQuestion = ref(0);
const questions = DATA;

const handleAnswer = (isCorrect) => {
  clearTimeout(timer.value);
  let nextQuestion = currentQuestion.value + 1;

  if (isCorrect) {
    score.value = score.value + 1;
  }

  if (nextQuestion < questions.length) {
    currentQuestion.value = nextQuestion;

    countDown.value = 30;
    countDownTimer();
  } else {
    showScore.value = true;
  }
};

const countDownTimer = () => {
  if (countDown.value > 0) {
    timer.value = setTimeout(() => {
      countDown.value -= 1;
      countDownTimer();
    }, 1000);
  } else {
    handleAnswer(false);
  }
};

const resetData = () => {
  setTimeout(() => {
    score.value = 0;
    showScore.value = false;
    currentQuestion.value = 0;

    countDown.value = 30;
    countDownTimer();
  }, 1000);
};

onMounted(() => {
  countDownTimer();
});
</script>
