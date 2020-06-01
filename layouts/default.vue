<template>
  <div class="app-layout">
    <div class="sidebar">
      <p>チャンネル一覽</p>
      <p v-for="channel in channels" :key="channel.id">
        <nuxt-link :to="`/channels/${channel.id}`">{{ channel.name }}</nuxt-link>
      </p>
      <p class="logout" v-if="isAuthenticated" v-on:click="logout">ログアウト</p>
    </div>
    <div class="main-content">
      <nuxt />
    </div>
  </div>
</template>

<script>
import { db, firebase } from '~/plugins/firebase'
import { mapActions } from 'vuex'

export default {
  data() {
    return {
      channels: []
    }
  },
  computed: {
    isAuthenticated() {
      return this.$store.getters.isAuthenticated
    }
  },
  methods: {
    ...mapActions(['setUser']),
    logout() {
      // firebaseからサインアウト＆vuexのユーザー情報からも削除
      firebase.auth().signOut()
        .then(() => {
          this.setUser(null)
          window.alert('ログアウトに成功')
        })
        .catch((e) => {
          window.alert('ログアウトに失敗しました')
          console.log(e)
        })
    }
  },
  mounted() {
    // 現在ログインしているユーザーを取得するには、Authオブジェクトでオブザーバーを設定することをオススメ
    // 詳しくは以下のドキュメントを参考にすること
    //   https://firebase.google.com/docs/auth/web/manage-users?hl=JA
    firebase.auth().onAuthStateChanged((user) => {
      if (user) {
        this.setUser(user)
      }
    })
    // データの取得情報に関しては以下のドキュメントに書かれています
    //    https://firebase.google.com/docs/firestore/query-data/get-data?hl=ja#get_multiple_documents_from_a_collection
    db.collection('channels').get()
      .then((querySnapshot) => {
        querySnapshot.forEach((doc) => {
          // スプレッド構文を使用して id を付加する
          //    https://qiita.com/akisx/items/682a4283c13fe336c547
          this.channels.push({ id: doc.id, ...doc.data() })
        })
        // データの確認用のコード
        // console.log(this.channels)
      })
  }
}
</script>

<style>
html {
  font-family: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: border-box;
  margin: 0;
}

.button--green {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #3b8070;
  color: #3b8070;
  text-decoration: none;
  padding: 10px 30px;
}

.button--green:hover {
  color: #fff;
  background-color: #3b8070;
}

.button--grey {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #35495e;
  color: #35495e;
  text-decoration: none;
  padding: 10px 30px;
  margin-left: 15px;
}

.button--grey:hover {
  color: #fff;
  background-color: #35495e;
}

.app-layout {
  /* flex：ある要素に定義するだけで、その直下の要素が並列になるスタイル */
  display: flex;
}

.sidebar {
  width: 300px;
  background: #4A4141;
  /*  
    vw の v は Viewport のイニシャル
    vw  ：ビューポートの幅に対する割あり
    vh  ：ビューポートの高さに対する割合
    vmin：ビューポートの幅と高さのうち、値が小さい方に対する割合
    vmax：ビューポートの幅と高さのうち、値が大きい方に対する割合

    iPhone 5S のビューポート幅をピクセルで表すと 320px な訳ですが、10vw はその 1/10 ということで32px が img の幅となります
  */
  height: 100vh;
  padding: 20px;
}

.sidebar p {
  color: #dddddd;
  padding-top: 4px;
}

.main-content {
  width: 100%;
  background: #F1F1F1;
  height: 100vh;
}

.logout {
  position: absolute;
  bottom: 10px;
  cursor: pointer;
}
</style>
