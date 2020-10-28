<template>
<div id="app">
  <Header :correct="correct" :total="total"></Header>
  <b-container class="bv-example-row">
    <b-row>
      <b-col sm="6" offset="3">
        <QuestionBox v-if="questions.length" :question="questions[index]" :next="next" :incrementCounter="incrementCounter">
        </QuestionBox>
      </b-col>
    </b-row>
  </b-container>
</div>
</template>

<script>
import axios from 'axios'
import QuestionBox from './components/QuestionBox.vue'
import Header from './components/Header.vue'

export default {
  name: 'App',
  components: {
    QuestionBox,
    Header
  },
  data() {
    return {
      url: 'https://opentdb.com/api.php?amount=10&difficulty=easy&type=multiple',
      questions: [],
      index: 0,
      correct:0,
      total:0
    }
  },
  methods: {
    next() {
      // Just loop for questions,
      // need to refresh page to get new questions
      if (this.index < 9) {
        this.index++
      } else {
        this.index = 0
      }
  },
  incrementCounter(isCorrect){
      if(isCorrect){
          this.correct++
      }
      this.total++
  }
  },
  mounted() {
    //do something after mounting vue instance
    axios
      .get(this.url)
      .then((response) => {
        this.questions = response.data.results
      })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
