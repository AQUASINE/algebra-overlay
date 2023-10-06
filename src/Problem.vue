<style>
.container__problem.correct {
  color: var(--color-green);
}

.container__problem.incorrect {
  color: var(--color-red);
}

.container__problem {
  display: flex;
  flex-direction: row;
  justify-content: end;
  margin-bottom: 5px;
}

.container__problem input {
  border-radius: 5px;
  width: 300px;
  margin-left: 30px;
  border: 1px solid white;
}

.container__problem.correct input {
  color: var(--color-green);
  border: 1px solid var(--color-green);
}

.container__problem.incorrect input {
  color: var(--color-red);
  border: 1px solid var(--color-red);
}

.container__center {
  display: flex;
  flex-direction: column;
  align-items: end;
  width: 800px;
}
</style>
<template>
  <div class="container__center">
    <div class="container__problem" :class="{correct: isCorrectAnswer, incorrect: input && !isCorrectAnswer}">
      {{ operand1 }} {{ problem.operator }} {{ operand2 }} = <input type="number"
                                                                    placeholder="?"
                                                                    v-model="input"/>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Problem',
  props: {
    problem: {}
  },
  emits: ['updateCorrect'],
  data() {
    return {
      input: ''
    }
  },
  computed: {
    isCorrectAnswer() {
      console.log(this.problem.operands.join(this.problem.operator))
      let answer = eval(this.problem.operands.join(this.problem.operator))
      return answer === this.input
    },
    operand1() {
      if (!this.problem) return
      return this.problem.operands[0]
    },
    operand2() {
      if (!this.problem) return
      return this.problem.operands[1]
    },
  },
  watch: {
    isCorrectAnswer() {
      console.log('isCorrectAnswer', this.isCorrectAnswer)
      this.$emit('updateCorrect', this.problem.id, this.isCorrectAnswer)
    }
  }

}
</script>