
<template>
  <div class="Layout">
    <div class="Messages">
      <message-item v-for="(message, key) in messages" :key="key" :message="message" />
    </div>
    <div class="MessageInput">
      <input class="TextInput" v-model="message" />
      <button class="SendButton" @click="send">
        <img src="@/assets/icons/send.svg" />
      </button>
    </div>
  </div>
</template>
<script>
import io from 'socket.io-client';
import MessageItem from '@/components/MessageItem.vue';

export default {
  name: 'layout',
  components: {
    MessageItem
  },
  data() {
    return {
      messages: [],
      message: '',
      socket: io('http://localhost:3000')
    }
  },
  mounted() {
    this.socket.on('new_message', (message) => {
      this.messages.push(message);
    })
  },
  methods: {
    send() {
      this.socket.emit('send-message', this.message);
      this.message = '';
    }
  }
}
</script>
<style lang="scss" scoped>
.Layout {
  width: 90vw;
  position: relative;

  .Messages {
    height: calc(100vh - 91px);
    padding: 10px 30px;
  }
  .TextInput {
    width: 100%;
    height: 50px;
    display: flex;
    align-items: center;
    border: none;
    padding: 10px 20px;
    font-size: 18px;
    border-top: 1px solid #d6d6d6;
    outline: none;
  }

  .SendButton {
    cursor: pointer;
    outline: none;
    position: absolute;
    right: 20px;
    bottom: 25px;
    width: 20px;
    height: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
    border: none;
    background: none;
  }
}
</style>