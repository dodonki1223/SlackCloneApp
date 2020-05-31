<template>
  <div class="chat-container">
    <div class="thumbnail-container">
      <img :src="message.user.thumbnail" />
    </div>
    <div class="message-container">
      <div class="user-name">{{ displayName }}</div>
      <div class="message">{{ message.text }}</div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['message'],
  computed: {
    displayName() {
      return "@" + this.message.user.name
      // 下記のように書くこともできる
      // return `@${this.message.user.name}`
    }
  }
}
</script>

// scoped はここで書いた css は他の .vue ファイルに一切影響を与えません
// index.vue にしか適用されないため他のファイルの css が邪魔してきてスタイルがぐちゃぐちゃ
// になることがないのです。これがコンポーネントの魅力の１つです
<style scoped>
.chat-container {
  display: flex;
  padding: 8px;
}

.thumbnail-container {
  margin-right: 16px;
}

.thumbnail-container img {
  width: 50px;
  height: 50px;
  border-radius: 10px;
}

.user-name {
  font-weight: bold;
}
</style>
