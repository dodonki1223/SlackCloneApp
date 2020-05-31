<template>
  <div class="container">
    <button v-on:click="login">ログイン</button>
  </div>
</template>

<script>
import { firebase } from '~/plugins/firebase'

export default {
  methods: {
  }
}
</script>

<style scoped>
.container {
  height: 100%;
}
</style>
