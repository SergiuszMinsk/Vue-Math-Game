<template>
  <div class="alert">
    <h3>{{ x }} + {{ y }} = ?</h3>

    <div class="buttons">
      <button class="btn btn-success btn--offsets" v-for="number in answers"
       @click="onAnswer(number)"
      >
        {{ number }}
      </button>
    </div>
    <answer-input
                 @input="updateAnswerVal"
                 @answer="onAnswer(getNumber)"
                 :isValid="isValid">
          <span>Your answer is: {{ answer }} </span>
    </answer-input>

  </div>
</template>

<script>
    import AnswerInput from './AnswerInput.vue'

    export default {
      name: 'question',
      props: ['settings'],
      data () {
          return {
            x: mtRand(this.settings.from, this.settings.to),
            y: mtRand(this.settings.from, this.settings.to),
            answer: '',
            isValid: true
          }
      },
      components: {
         AnswerInput
      },
      computed: {
        good () {
          return this.x + this.y;
        },

        getNumber() {
          return +this.answer.replace(/\s/g, '');
        },

        answers () {
          let res = [this.good];

          while (res.length < this.settings.variants) {
            let num = mtRand(this.good - this.settings.range, this.good + this.settings.range);
            if (!~res.indexOf(num)) {
              res.push(num);
            }
          }

          return res.sort(() => Math.random() > .5);
        }
      },
      methods: {
        validateNumbers () {
          let noSpacesAnswer = this.answer.replace(/\s/g, '');
          this.isValid = !isNaN(Number(noSpacesAnswer));
        },
        updateAnswerVal (value) {
          this.answer = value;
          this.validateNumbers();
        },
        onAnswer (num) {
          if (num === this.good) {
            this.$emit('success');
          } else {
            this.$emit('error', `${this.x} + ${this.y} = ${this.good}!`);
          }
        },
      }
    }

    function mtRand(min, max) {
      let diff = max - min;
      return Math.floor(Math.random() * (diff + 1)) + min;
    }
</script>

<style scoped>
  .buttons {
    display: flex;
  }

  .btn--offsets {
    margin: 25px 25px 0 0;
  }
</style>
