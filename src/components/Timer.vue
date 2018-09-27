<template>
  <span>{{current}}</span>
</template>

<script>
    export default {
      name: 'timer',
      props: {
        interval: {
          type: Number,
          default: 1
        },
        initial: {
          type: Number
        }
      },

      data() {
        return {
          current: this.initial,
          timerInterval: null
        }
      },

      methods: {
        onInterval() {
          this.current--;
          if (this.current <= 0) {
            clearInterval(this.timerInterval);
            this.$emit('unanswered');
          }
        },
      },

      mounted() {
        this.timerInterval = setInterval(this.onInterval, this.interval * 1000)
      }
    }
</script>

<style scoped>

</style>
