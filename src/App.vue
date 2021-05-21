<template>
  <div>
    <ScoreBoard
      :correctCount="this.correctCount"
      :incorrectCount="this.incorrectCount"
    />
    <template v-if="this.question">
      <h1 v-html="this.question"></h1>
      <div v-for="(answer, index) in this.answers" :key="index">
        <input
          :disabled="this.answerSubmitted"
          type="radio"
          name="options"
          :value="answer"
          v-model="this.chosenAnswer"
        />
        <label v-html="answer"></label>
      </div>
      <br />
      <button
        v-if="!this.answerSubmitted"
        @click="this.submitAnswer()"
        class="send"
        type="button"
      >
        Submit
      </button>

      <button
        v-if="this.answerSubmitted"
        @click="this.getNewQuestion()"
        class="send"
        type="button"
      >
        Next
      </button>

      <section v-if="answerSubmitted" class="result">
        <h4
          class="incorrect"
          v-if="
            this.chosenAnswer != this.correctAnswer &&
              this.chosenAnswer != 'undefined'
          "
          v-html="'Incorrect, the correct answer is ' + this.correctAnswer"
        ></h4>
        <h4 class="correct" v-else v-html="' Correct.'"></h4>
      </section>
    </template>
  </div>
</template>

<script>
import ScoreBoard from '@/components/ScoreBoard.vue'

export default {
  name: 'App',
  components: {
    ScoreBoard,
  },
  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosenAnswer: undefined,
      answerSubmitted: false,
      correctCount: 0,
      incorrectCount: 0,
    }
  },
  computed: {
    answers() {
      var answers = JSON.parse(JSON.stringify(this.incorrectAnswers))
      answers.splice(
        Math.round(Math.random() * answers.length),
        0,
        this.correctAnswer
      )
      return answers
    },
  },
  methods: {
    submitAnswer() {
      if (!this.chosenAnswer) alert('Pick one of the options')
      else {
        this.answerSubmitted = true
        if (this.chosenAnswer == this.correctAnswer) {
          this.correctCount++
        } else this.incorrectCount++
      }
    },
    getNewQuestion() {
      this.answerSubmitted = false
      this.chosenAnswer = undefined
      this.question = undefined
      this.axios({
        method: 'GET',
        url: 'https://opentdb.com/api.php?amount=1&category=18',
        timeout: 1000,
      })
        .then((response) => {
          this.question = response.data.results[0].question
          this.incorrectAnswers = response.data.results[0].incorrect_answers
          this.correctAnswer = response.data.results[0].correct_answer
        })
        .catch((error) => {
          console.error(error)
          //if timeout request again
          this.getNewQuestion()
        })
    },
  },
  created() {
    if (!this.answerSubmitted) this.getNewQuestion()
  },
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

  input[type='radio'] {
    margin: 12px 4px;
  }

  button.send {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: #fff;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    cursor: pointer;
  }
  .correct {
    color: green;
  }
  .incorrect {
    color: red;
  }
}
</style>
