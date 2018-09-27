<template>
  <div class="alert alert-secondary">
    <h3>Are you ready?</h3>
    <button class="btn btn-success" @click="$emit('onStart')">Let's go</button>
  </div>
</template>

<script>
    export default {
        name: 'start-screen'
    }
</script>

<style scoped>
  .alert {
    text-align: center;
  }

  h3, .btn {
    margin: 30px 0;
  }
</style>
