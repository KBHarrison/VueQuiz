<template>
<div>
  <b-jumbotron header="BootstrapVue" lead="Bootstrap v4 Components for Vue.js 2">
    <template v-slot:header>Very exciting quiz!!</template>

    <template v-slot:lead>
     {{currentQuestion.question}}
    </template>

    <hr class="my-4">
    <b-list-group>
    <b-list-group-item 
        v-for="(answer, index) in shuffledAnswers" 
        :key="index"
        @click="selectAnswer(index)"
        :class="[selectedIndex === index ? 'selected' : ''] "
        >
        {{ answer }}
    </b-list-group-item>
    </b-list-group>
    <b-button variant="primary" @click="back" >Back</b-button>
    <b-button variant="success" @click="next" href="#">Next</b-button>
  </b-jumbotron>
</div></template>

<script>
import _ from lodash
export default {
    props: {
        currentQuestion: Object,
        next: Function,
        back: Function
    },
    data() {
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: []
        }
    },
    methods: {
        selectAnswer(index) {
            this.selectedIndex = index
            if (this.correctindex === index) {
                console.log("You are correct!!")
            }
        }
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers];
            let num = Math.floor(Math.random() * Math.floor(4));
            answers.push(this.currentQuestion.correct_answer);
            // this.correctIndex = num;
            return answers
        },
        shuffleAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
        }
    },
    watch: {
        currentQuestion() {
            this.selectedIndex = null
            this.shuffleAnswers()
        }
    }
}
</script>

<style scoped>
.list-group {
    margin-bottom: 15px;
}
.list-group-item:hover {
    background: #EEE;
    cursor: pointer;
}
.selected {
    background-color: lightblue;
}
.correct {
    background-color: green;
}
.incorrect {
    background-color: red;
}
.btn {
    margin: 0 5px;
}
</style>