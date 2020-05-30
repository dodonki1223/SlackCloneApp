<template>
  <div class="container">
    <div class="chats-layout">
      <messages />
    </div>
    <div class="input-layout">
      <div class="input-container">
        <textarea></textarea>
      </div>
    </div>
  </div>
</template>

<script>
import Messages from '~/components/Messages.vue'

export default {
  components: {
    Messages
  }
}
</script>

<style scoped>
.container {
  height: 100%;
}

.chats-layout {
  /* 
    内容がボックスに収まらない場合、収まらない部分はボックスからはみ出さない 
    内容が収まらない場合には、スクロールバーなどが表示される
  */
  overflow: scroll;
  height: 90%;
}

.input-layout {
  height: 10%;
}

.input-container {
  padding: 10px;
  height: 100%;
}

textarea {
  width: 100%;
  height: 100%;
}
</style>
