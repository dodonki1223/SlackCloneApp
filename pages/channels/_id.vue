<template>
  <div class="container">
    <div class="chats-layout">
      <messages :messages="messages" />
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
  },
  data() {
    return {
      messages: []
    }
  },
  mounted() {
    const channelId = this.$route.params.id
    /*
      -------------------------------------------------------------------------------------------------
      db.collection('channels').doc(channelId).collection('messages')
        .onSnapshot((snapshot) => {
        })
      -------------------------------------------------------------------------------------------------
        onSnapshot を collection('messages') に対して実行している。これは 「messages collection に変更
        があったら何か処理を実行しますよ」ということを意味しています
      -------------------------------------------------------------------------------------------------
      snapshot.docChanges().forEach((change) => {})
      -------------------------------------------------------------------------------------------------
        collection に変化があった時に snapshot がコールバック関数に渡されます。snapshot はデータベースの
        コピーのようなものです
      -------------------------------------------------------------------------------------------------
      const doc = change.doc
      -------------------------------------------------------------------------------------------------
        change.doc で doc が取得できます
      -------------------------------------------------------------------------------------------------
      if (change.type === 'added') {}
      -------------------------------------------------------------------------------------------------
        firestore の変更といってもいろいろあります。データの追加かもしれないし、変更かもしれない、
        削除かもしれないですね
        そのタイプについてはこちらを:https://firebase.google.com/docs/firestore/query-data/listen?hl=ja
    */
    db.collection('channels').doc(channelId).collection('messages')
      .onSnapshot((snapshot) => {
        snapshot.docChanges().forEach((change) => {
          const doc = change.doc
          if (change.type === 'added') {
            this.messages.push({ id: doc.id, ...doc.data() })
          }
        })
      })
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
