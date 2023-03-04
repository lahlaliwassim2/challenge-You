<script setup>
import { ref, computed } from 'vue'
const questions = ref([
  {
	question: 'What is Vue?',
	answer: 0,
	options: [
		'A framework',
		'A library',
		'A type of hat'
	],
	selected: null
  },
  {
	question: 'What is Vuex used for?',
	answer: 2,
	options: [
		'Eating a delicious snack',
		'Viewing things',
		'State management'
	],
	selected: null
  },
  {
	question: 'What is Vue Router?',
	answer: 1,
	options: [
		'An ice cream maker',
		'A routing library for Vue',
		'Burger sauce'
	],
	selected: null
  }
])
const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
	let value = 0
	questions.value.map(q => {
		if (q.selected != null && q.answer == q.selected) {
			console.log('correct');
			value++
		}
	})
	return value
})
const getCurrentQuestion = computed(() => {
	let question = questions.value[currentQuestion.value]
	question.index = currentQuestion.value
	return question
})
const SetAnswer = (e) => {
	questions.value[currentQuestion.value].selected = e.target.value
	e.target.value = null
}
const NextQuestion = () => {
	if (currentQuestion.value < questions.value.length - 1) {
		currentQuestion.value++
		return
	}
	
	quizCompleted.value = true
}
</script>

<template>
	<main class="app">
		<h1>The Quiz</h1>
		
		<section class="quiz" v-if="!quizCompleted">
			<div class="quiz-info">
				<span class="question">{{ getCurrentQuestion.question }}</span>
				<span class="score">Score {{ score }}/{{ questions.length }}</span>
			</div>
			
			<div class="options">
				<label 
					v-for="(option, index) in getCurrentQuestion.options" 
          :key="index"
					:for="'option' + index" 
					:class="`option ${
						getCurrentQuestion.selected == index 
							? index == getCurrentQuestion.answer 
								? 'correct' 
								: 'wrong'
							: ''
					} ${
						getCurrentQuestion.selected != null &&
						index != getCurrentQuestion.selected
							? 'disabled'
							: ''
					}`">
					<input 
						type="radio" 
						:id="'option' + index" 
						:name="getCurrentQuestion.index" 
						:value="index" 
						v-model="getCurrentQuestion.selected" 
						:disabled="getCurrentQuestion.selected"
						@change="SetAnswer" 
					/>
					<span>{{ option }}</span>
				</label>
			</div>
			
			<button 
				@click="NextQuestion" 
				:disabled="!getCurrentQuestion.selected">
				{{ 
					getCurrentQuestion.index == questions.length - 1 
						? "c'est terminer !!" 
						: getCurrentQuestion.selected == null
							? 'vous devez selectionner un reponse'
							: 'nouvelle question'
				}}
			</button>
		</section>

		<section v-else>
			<h2>You have finished the quiz!</h2>
			<p>Your score is {{ score }}/{{ questions.length }}</p>
		</section>
	</main>
</template>

<style>

.app {
  
  display: flex;
  flex-direction: column;
  align-items: start;
  justify-content: center;
  height: 100vh;
  font-family: Arial, sans-serif;
}

h1 {
  font-size: 2rem;
  margin-left: 50%;
  margin-bottom: 7rem;
}

.quiz {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.quiz-info {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 2rem;
}

.question {
  font-size: 3Yrem;

  margin-bottom: 1rem;
  text-align: center;
}

.score {
  font-size: 1.2rem;
  color: #777;
}

.options {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-bottom: 2rem;
}

.option {
  display: flex;
  align-items: center;
  margin-bottom: 1rem;
  padding: 0.5rem;
  border: 2px solid #ccc;
  border-radius: 0.5rem;
  cursor: pointer;
}

.option:hover {
  background-color: #eee;
}

.option.disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.option.correct {
  border-color: green;
}

.option.wrong {
  border-color: red;
}

button {
  font-size: 1.2rem;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 0.5rem;
  background-color: #007bff;
  color: white;
  cursor: pointer;
}

button:hover {
  background-color: #0069d9;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
</style>
