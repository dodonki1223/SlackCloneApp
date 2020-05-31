<template>
  <div class="input-container">
    <textarea v-model="text" v-on:keydown.enter="addMessage"></textarea>
  </div>
</template>

<script>
import { db } from '~/plugins/firebase' 

export default {
  data() {
    return {
      text: null
    }
  },
  methods: {
    addMessage(event) {
      /* 
        v-on:keydown.enterでは日本語入力中のEnterのkeyCodeと通常のEnterのkeyCodeが一緒の扱いになってしまう
        日本語入力中の時は return して何も処理しないようにする
        以下の記事を参考にするとよい
          https://qiita.com/TsukasaGR/items/22b306cb819bc7164bd7
      */
      if (this.keyDownForJPConversion(event)) { return }

      const channelId = this.$route.params.id
      /*
        firestore の document にはデータが更新された日時を管理する機能はありません
        つまり、データを作成時に作成した時の日時を入力しておき、それ順に並び替えて表示する必要があります
        firestore には時間を管理する型が用意されているが、ちょっと扱いづらい所があるのでちょっと特殊なことをやります
        getTime() メソッドを実行すると数字のみでその日時の値を取得できます
       */
      db.collection('channels').doc(channelId).collection('messages').add({
        text: this.text, 
        createdAt: new Date().getTime() 
      }).then(() => {
          this.text = null
        })
    },
    keyDownForJPConversion(event) {
      const codeForConversion = 229
      return event.keyCode === codeForConversion
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
