<template>
  <div class="input-container">
    <!--  
      v-on は @ と省略できるので、以下のように書くこともできます
      <textarea @click="login"></textarea>
    -->
    <textarea v-on:click="login"></textarea>
  </div>
</template>

<script>
export default {
  methods: {
    login() {
      window.alert('ログインしろよ')
    }
  }
}
</script>

<style scoped>
.input-container {
  padding: 10px;
  height: 100%;
}

textarea {
  width: 100%;
  height: 100%;
}
</style>
