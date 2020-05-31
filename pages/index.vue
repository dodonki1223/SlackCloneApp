<template>
  <div class="container">
    <div class="chats-layout">
      <messages />
    </div>
    <div class="input-layout">
      <!--  
        「めんどくさい！」って思うかもしれませんが、Vueの開発では以外にこれが重要です
        HTML/CSS だけなら良いですが、JavaScriptものちほど本格的に登場してきます
        そのときにコードが非常に複雑になることが多いです
        Componentで分けていれば、Componentごとに処理が別れているのでコードが理解しやすくなります
      -->
      <chat-form />
    </div>
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
</style>
