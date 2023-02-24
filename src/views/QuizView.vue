<script setup>
import Question from "../components/Question.vue";
import QuizHeader from "../components/QuizHeader.vue";
import { useRoute } from "vue-router";
import { ref, watch, computed } from "vue";
import quizes from "../data/quizes.json"

const route = useRoute();
const quizId = parseInt(route.params.id);
const quiz = quizes.find(q => q.id === quizId)
const currentQuestionIndex = ref(0);
const numberOfCorrectAnswers = ref(0);
const onOptionSelected = (isCorrect) => {
  if(isCorrect) numberOfCorrectAnswers.value++
  currentQuestionIndex.value++
}
// const questionStatus = ref(`${currentQuestionIndex.value}/${quiz.questions.length}`)
// watch(()=> currentQuestionIndex.value, () => {
//   questionStatus.value = `${currentQuestionIndex.value}/${quiz.questions.length}`
// })
const questionStatus = computed(() => `${currentQuestionIndex.value}/${quiz.questions.length}`)
const barPercentage = computed(() => `${currentQuestionIndex.value/quiz.questions.length * 100}%`)
</script>
<template>
  <div v-if="currentQuestionIndex < quiz.questions.length">
    <QuizHeader 
      :questionStatus="questionStatus"
      :barPercentage="barPercentage"
    ></QuizHeader>
    <Question 
    :question="quiz.questions[currentQuestionIndex]"
    @selectOption="onOptionSelected"
    />
  </div>
  <div v-else class="resultPage">
    <h1>Quiz Finished</h1>
    <h2 class="congrat" v-if="numberOfCorrectAnswers == quiz.questions.length">Congratulations !!</h2>
    <h2>You got {{numberOfCorrectAnswers}} out of {{quiz.questions.length}} correct</h2>
    <button class="homeBtn"><router-link to="/">Back to Home</router-link></button>
  </div>
</template>

<style scoped>
.resultPage{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}
.homeBtn{
  text-decoration: none;
  margin-top: 20px;
  padding: 10px;
  border: none;
  background-color: rgb(156, 153, 153);
  color: white;
  border-radius: 5px;
  cursor: pointer;
  font-size: 20px;

}
.congrat{
  color: green;
  font-size: 36px;
  font-weight: bold;
}
</style>