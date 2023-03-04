<script setup>
import { ref, computed } from "vue";
const questions = ref([
  {
    question: "What is Vue?",
    answer: 0,
    options: ["A framework", "A library", "A type of hat"],
    selected: null,
  },
  {
    question: "What is Vuex used for?",
    answer: 2,
    options: ["Eating a delicious snack", "Viewing things", "State management"],
    selected: null,
  },
  {
    question: "What is Vue Router?",
    answer: 1,
    options: [
      "An ice cream maker",
      "A routing library for Vue",
      "Burger sauce",
    ],
    selected: null,
  },
]);
const currentQuestion = ref(0);
const quizCompleted = ref(false);
const score = computed(() => {
  let value = 0;
  questions.value.map((q) => {
    if (q.selected == q.answer) {
      value++;
    }
  });
  return value;
});
const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value];
  questions.index = currentQuestion.value;
  return question;
});

const setAnswer = (evt) => {
  questions.value[currentQuestion.value].selected = evt.target.value;
  evt.target.value = null;
};

const nextQuestion = () => {
  if (currentQuestion.value < questions.value.lenght - 1) {
    currentQuestion.value++;
  } else {
    quizCompleted.value = true;
  }
};
</script>

<template>
  <main class="app">
    <h1>Quiz</h1>
    <section class="quiz">
      <div class="quiz-info">
        <span class="question">{{ getCurrentQuestion.question }}</span>
        <span class="score">Score : {{ score }} / {{ questions.length }}</span>
      </div>
      <div class="options">
        <label v-for="(options , index) in getCurrentQuestion.options"
        :key="index"
        :class="`option ${
          getCurrentQuestion.selected==index
            ? index == getCurrentQuestion.selected.answer
              ? 'correct'
              :'wrong'
            : ''
        } ${
          getCurrentQuestion.selected != null && 
          index != getCurrentQuestion.selected
            ? 'disabled'
            : ''
        }`"
        >
          <input type="radio" 
          :name="getCurrentQuestion.index" 
          :value="index" 
          
          />
        </label>
      </div>
    </section>
  </main>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Montsrrat", sans-serif;
}

body {
  background-color: bisque;
  color: antiquewhite;
}
</style>
