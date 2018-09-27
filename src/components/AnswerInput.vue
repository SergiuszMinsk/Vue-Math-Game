<template>
  <div class="answer-input">
    <div class="answer-input__item">

      <input
        @keyup.enter="onAnswer"
        @input="updateValue($event)"
        placeholder="Write an answer"/>

      <div v-if="isValid" class="answer-input__item-actions">
        <slot></slot>
        <button class="btn btn-success" @click="onAnswer">Submit answer!</button>
      </div>
      <span v-else class="warning-msg">Only numbers required!</span>
    </div>
  </div>
</template>

<script>
    export default {
        name: 'AnswerInput',
        props: {
          isValid: Boolean
        },
        data () {
            return {

            }
        },
        methods: {
          onAnswer () {
            this.$emit('answer');
          },
          updateValue: function (event) {
            this.$emit('input', event.target.value)
          }
        }
    }
</script>

<style scoped>
  .warning-msg {
    color: red;
  }

  .answer-input__item-actions{
    margin-top: 20px;
  }

  .answer-input__item {
    display: flex;
    flex-direction: column;
  }
  .answer-input {
    margin-top: 50px;
    display: flex;
    justify-content: center;
  }
</style>
