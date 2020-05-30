<template>
  <!-- 
    読みやすいコードとは読者に読ませないコード 
    以下のコードは読まないと意味不明です……のちにリファクタする
  -->
  <div class="container">
    <div class="chats-container">

      <div class="chat-container">
        <div class="thumbnail-container">
          <img src="https://pbs.twimg.com/profile_images/1131882065337602049/0JRUGd3S_400x400.jpg" />
        </div>
        <div class="message-container">
          <div class="user-name">cohki0305</div>
          <div class="message">今日も良い天気ですね</div>
        </div>
      </div>

      <div class="chat-container">
        <div class="thumbnail-container">
          <img src="https://pbs.twimg.com/profile_images/1131882065337602049/0JRUGd3S_400x400.jpg" />
        </div>
        <div class="message-container">
          <div class="user-name">cohki0305</div>
          <div class="message">そうですねー</div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>

export default {
  components: {
  }
}
</script>

// scoped はここで書いた css は他の .vue ファイルに一切影響を与えません
// index.vue にしか適用されないため他のファイルの css が邪魔してきてスタイルがぐちゃぐちゃ
// になることがないのです。これがコンポーネントの魅力の１つです
<style scoped>
.chats-container {
  padding: 16px;
}

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
