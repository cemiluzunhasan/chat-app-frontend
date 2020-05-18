<template>
  <div class="MessageItem">
    <p class="MessageText">{{ message }}</p>
  </div>
</template>
<script>
export default {
  name: 'message-item',
  props: {
    message: {
      type: String
    }
  }
}
</script>
<style scoped>

</style>