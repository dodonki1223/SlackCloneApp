<template>
  <div class="container">
    <button v-on:click="login">ログイン</button>
  </div>
</template>

<script>
import { firebase } from '~/plugins/firebase'

export default {
  methods: {
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
          console.log(user)
        }).catch((error) => {
          window.alert(error)
        })
    }
  }
}
</script>

<style scoped>
.container {
  height: 100%;
}
</style>
