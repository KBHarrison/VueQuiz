<template>
  <div id="app">
    <Header 
    :numberOfCorrectAnswers="numCorrect"
    :numTotal="numTotal"
    :index="index" />
    <b-container>
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
          :currentQuestion="questions[index]"
          :next="next" 
          :back="back"
          :increment="increment"
          :index="index"
          v-if="questions.length"/>
        </b-col>
      </b-row>
    </b-container>
    <img v-if="!questions.length" alt="Vue logo" src="./assets/logo.png">
  </div>
</template>

<script>
// AT 1:29 in Learn Vue.js - Full Course for Beginners - 2019

import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  methods: {
    next() {
      this.index++;
    },
    back() {
      this.index--;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
      }
      this.numTotal++
      this.questions[this.index]['previouslyAnswered'] = true
    }
  },
  mounted: function() {
    fetch("https://opentdb.com/api.php?amount=10&type=multiple", {
      method: "get"
    })
    .then ((response) => {
      return response.json()
    })
    .then (jsonData => {
      this.questions = jsonData.results
      return this.questions
    })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
