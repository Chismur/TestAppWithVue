<template>
<div id="question-box">
  <b-jumbotron>

    <template v-slot:lead>
      {{question.question}}
    </template>

    <hr class="my-4">

    <b-list-group>
      <b-list-group-item v-for="(answer, index) in answers" :key="answer" @click="selectAnswer(index)" :class="selectClass(selectedIndex, index)">
        {{answer}}
      </b-list-group-item>
    </b-list-group>

    <b-button variant="primary" @click="submit" :disabled="selectedIndex === null || isAnswered">Submit</b-button>
    <b-button variant="success" @click="next" :disabled="!isAnswered">Next</b-button>
  </b-jumbotron>
</div>
</template>
<script>
import _ from 'lodash'
export default {
  name: "QuestionBox",
  props: {
    question: {
      type: Object
    },
    next: Function,
    incrementCounter:Function
  },
  data() {
    return {
      selectedIndex: null,
      correctAnswerIndex: null,
      shuffledAnswers: [],
      isAnswered: false,
      isCorrect:false
    }
  },
  watch: {
    question: {
      immediate: true,
      handler() {
        this.selectedIndex = null,
          this.isAnswered = false,
          this.shuffleAnswers()
      }
    }
  },
  computed: {
    answers() {
      let answers = [...this.question.incorrect_answers, this.question.correct_answer]
      return answers
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    submit() {
        this.isCorrect = false
        if(this.selectedIndex === this.correctAnswerIndex){
            this.isCorrect = true
        }
      this.isAnswered = true
      this.incrementCounter(this.isCorrect)
    },
    selectClass(selectedIndex, index) {
      return !this.isAnswered && selectedIndex === index ? 'selected' :
        this.isAnswered && index === this.correctAnswerIndex ? 'correct' :
        this.isAnswered && selectedIndex === index && selectedIndex !== this.correctAnswerIndex ? 'incorrect' : ''
    },
    shuffleAnswers() {
      let a = [...this.question.incorrect_answers, this.question.correct_answer]
      this.shuffledAnswers = _.shuffle(a)
      this.correctAnswerIndex = this.shuffledAnswers.indexOf(this.question.correct_answer)
    }
  }
}
</script>
<style scoped>
.list-group {
  margin-bottom: 20px;
}

.list-group-item:hover {
  background: lightcyan;
  cursor: pointer;

}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: red;
}

.selected {
  background-color: lightblue;
}

.btn {
  margin: 0 5px
}
</style>
