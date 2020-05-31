<template>
  <div class="container">
  </div>
</template>

<script>
import Messages from '~/components/Messages.vue'
import ChatForm from '~/components/ChatForm.vue'
import { db } from '~/plugins/firebase'

export default {
  components: {
    Messages,
    ChatForm
  }
}
</script>

<style scoped>
.container {
  height: 100%;
}
</style>
