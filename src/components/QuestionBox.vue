<template>
<div>
  <b-jumbotron header="BootstrapVue" lead="Bootstrap v4 Components for Vue.js 2">
    <template v-slot:header>Quiz</template>

    <template v-slot:lead>
     {{decoder(currentQuestion.question)}}
     <p class="correct" v-if="correct === true">Correct!</p>
     <p class="incorrect" v-if="correct === false">Incorrect. The answer is {{shuffledAnswers[correctIndex]}}</p>
     <p class="answered" v-if="previouslyAnswered">This question has already been answered. The correct answer is {{ shuffledAnswers[correctIndex] }}</p>
    </template>

    <hr class="my-4">
    <b-list-group>
    <b-list-group-item 
        v-for="(answer, index) in shuffledAnswers" 
        :key="index"
        @click="selectAnswer(index)"
        :class="[selectedIndex === index ? 'selected' : ''] "
        >
        {{ decoder(answer) }}
    </b-list-group-item>
    </b-list-group>
        <b-button id="back" variant="success" :disabled="index === 0" @click="back" >Back</b-button>
        <b-button variant="primary" 
        :disabled="selectedIndex === null || answered || this.currentQuestion['previouslyAnswered']" 
        @click="submitAnswer" >Submit</b-button>
        <b-button id="next" :disabled="index === 9" variant="success" @click="next">Next</b-button>
  </b-jumbotron>
</div></template>

<script>
import _ from 'lodash'
export default {
    props: {
        currentQuestion: Object,
        next: Function,
        back: Function,
        increment: Function,
        index: Number
    },
    data() {
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: [],
            answered: false,
            correct: null,
            previouslyAnswered: null

        }
    },
    methods: {
        selectAnswer(index) {
            this.selectedIndex = index
        },
        shuffleAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
            return true
        },
        submitAnswer() {
            let isCorrect = false;
            this.answered = true;
            if (this.selectedIndex === this.correctIndex) {
                isCorrect = true
                this.correct = true
            } else {
                this.correct = false
            }
            this.increment(isCorrect)
        },
        decoder (str) {
            var textArea = document.createElement('textarea');
            textArea.innerHTML = str;
            return textArea.value;
        }
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers];
            // let num = Math.floor(Math.random() * Math.floor(4));
            answers.push(this.currentQuestion.correct_answer);
            // num = 5
            return answers
        },
    },
    watch: {
        currentQuestion:  {
            immediate: true,
            handler() {
                this.selectedIndex = null
                this.answered = false
                this.shuffleAnswers()    
                this.correct = null
                if (this.currentQuestion['previouslyAnswered']) {
                    this.previouslyAnswered = true
                } else {
                    this.previouslyAnswered = false
                }        
            }
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
    background-color: lightgreen;
}
.incorrect {
    background-color: red;
}
.btn {
    margin: 0 5px;
}
#back {
    float: left;
}
#next {
    float: right;
}
.answered {
    color: red;
    font-size: 15px;
}

</style>