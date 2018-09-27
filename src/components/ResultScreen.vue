<template>
  <div class="alert alert-primary">
    <h3>Success: {{ stats.success }}</h3>
    <h3>Error: {{ stats.error }}</h3>
    <h3>Unanswered: {{ stats.unanswered }}</h3>
    <hr>
    <button class="btn btn-primary" @click="$emit('repeat')">Repeat</button>
    <button class="btn btn-primary" @click="onNextLevel" v-if="hasNextLevel">Next level</button>
  </div>
</template>

<script>
  export default {
    name: 'result-screen',
    props: ['stats', 'hasNextLevel'],
    methods: {
      onNextLevel() {
        this.$emit('nextLevel');
      }
    }
  }
</script>

<style scoped>

</style>
