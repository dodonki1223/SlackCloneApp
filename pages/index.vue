<template>
  <!-- 
    読みやすいコードとは読者に読ませないコード 
    以下のコードは読まないと意味不明です……のちにリファクタする
  -->
  <div class="container">
    <messages />
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

<style>
</style>
