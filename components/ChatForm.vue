<template>
  <div class="input-container">
    <textarea v-model="text" v-on:click="openLoginModal" v-on:keydown.enter="addMessage"></textarea>
    <!--  
      visibleに .sync を追加しないとモーダルを閉じることができない
      ChatForm.vueにおいて dialogVisible が変更されてたことが ElDialog の
      コンポーネントに伝わっていないのが原因
      詳しくは下記記事を参考にするにこと
        https://qiita.com/harhogefoo/items/7232508db1f07e6b1859
    -->
    <el-dialog
      title=""
      :visible.sync="dialogVisible"
      width="30%">
      <div class="image-container">
        <img src="~/assets/google_sign_in.png" v-on:click="login">
      </div>
    </el-dialog>
  </div>
</template>

<script>
import { db, firebase } from '~/plugins/firebase' 

import Vue from 'vue'
import { mapMutations } from 'vuex'
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'
Vue.use(ElementUI)

export default {
  data() {
    return {
      dialogVisible: false,
      text: null
    }
  },
  methods: {
    // スプレッド構文を使用する
    ...mapMutations(['setUser']),
    openLoginModal() {
      this.dialogVisible = true
    },
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
    },
    login() {
      // Provider を定義する
      //   Firebaseにはいろいろなログイン方法があります。なのでどのログイン方法を使うのかを指定してあげます
      const provider = new firebase.auth.GoogleAuthProvider()
      // firebase.auth の signIn メソッドを実行する
      // provider を引数にして signIn メソッドを実行してあげる
      firebase.auth().signInWithPopup(provider)
        .then((result) => {
          // then と catch については以下の記事を参照するとよい
          //   https://rightcode.co.jp/blog/information-technology/javascript-promise
          const user = result.user
          // vuex の mutations の setUser メソッドを実行している
          // index.jsのexport const strict = false を記載しているのはVuexの思想に則っていないで Action を
          // 使わないで State を変更しているからです
          // 詳しくは以下の記事を参考にしてください
          //   https://qiita.com/frost_star/items/4620957fce888150e4cc
          this.setUser(user)
          this.dialogVisible = false
        }).catch((error) => {
          window.alert(error)
        })
    }
  }
}
</script>

<style scoped>
.image-container {
  display: flex;
  justify-content: center
}

img {
  width: 70%;
  cursor: pointer;
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
