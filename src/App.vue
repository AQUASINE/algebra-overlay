<script>
import Problem from "./Problem.vue";

export default {
  name: 'App',
  components: {Problem},
  data() {
    return {
      problems: [
        {
          operator: '+',
          operands: [1, 2],
        },
        {
          operator: '+',
          operands: [1, 2],
        },
        {
          operator: '+',
          operands: [1, 2],
        },
      ],
      problemsCorrect: [
        false,
        false,
        false,
      ],
      timeElapsed: 0.0,
    };
  },
  created() {
    this.generateProblems();
    setInterval(() => {
      this.timeElapsed += 0.01
    }, 10)
  },
  methods: {
    generateProblems() {
      // make 3 problems, can be +, -, *, /
      // operands are 2-10 unless /, then the second operand is 2-10 and the first operand is the product of the second operand and a number 2-10

      let operators = ['+', '-', '*', '/']


      let problems = []

      for (let i = 0; i < 3; i++) {
        let operator = operators[Math.floor(Math.random() * operators.length)]

        let operand1;
        let operand2;
        if (operator === '*' || operator === '/') {
          operand1 = Math.floor(Math.random() * 11) + 2;
          operand2 = Math.floor(Math.random() * 11) + 2;
        } else {
          operand1 = Math.floor(Math.random() * 20) + 2;
          operand2 = Math.floor(Math.random() * 20) + 2;
        }

        if (operator === '/') {
          operand1 = operand1 * operand2
        }

        problems.push({
          id: i,
          operator,
          operands: [operand1, operand2]
        })
      }

      this.problems = problems
    },
    updateCorrect(id, correct) {
      console.log('updateCorrect', id, correct)
      this.problemsCorrect[id] = correct
    },
  },
  computed: {
    passed() {
      return this.problemsCorrect.every(problem => problem)
    },
    formattedTimeElapsed() {
      return this.timeElapsed.toFixed(2)
    }
  }
};
</script>

<template>
  <div class="container__problems">
    <div class="wrapper__problems" v-if="!passed">
      <div class="info__time-elapsed">
        QUICK! The clock is ticking!
      </div>
      <div class="info__seconds">
        You have lost {{ formattedTimeElapsed }} seconds
      </div>
      <Problem :problem="problem" v-for="problem in problems" @updateCorrect="updateCorrect"/>
    </div>
    <div class="info-passed" v-if="passed">
      PASSED
    </div>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

.info-passed {
  font-size: 3em;
  color: var(--color-green);
  text-align: center;
  font-weight: bold;
  animation: disappear 1s ease-in-out forwards;
}

@keyframes disappear {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

.container__problems {
  display: flex;
  flex-direction: column;
  align-items: end;
  margin-top: 100px;
}

.wrapper__problems {
  display: flex;
  flex-direction: column;
  align-items: end;
  width: 800px;
  background-color: #1a1a1a;
  padding: 30px;
  border-radius: 10px;
  border: 1px solid white;
}

.info__time-elapsed {
  font-size: 0.5rem;
  animation: pulse 0.2s steps(3) infinite, grow 0.2s ease-in-out infinite alternate;
  width: 100%;
}

@keyframes pulse {
  0% {
    color: red;
  }
  50% {
    color: #ffff00;
  }
  100% {
    color: var(--color-red);
  }
}

@keyframes grow {
  0% {
    transform: scale(1);
  }
  100% {
    transform: scale(1.1);
  }
}

.info__seconds {
  font-size: 0.5rem;
  width: 100%;
  color: var(--color-red);
  text-align: center;
  font-weight: bold;
}
</style>
