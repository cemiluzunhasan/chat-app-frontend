<template>
  <div class="LoginCard">
      <input name="username" type="text" placeholder="Username">
      <input name="password" type="password" placeholder="Password">
      <button class="LoginButton">Login</button>
      <button class="RegisterButton">Register</button>
    </div>  
</template>
<script>
import { generateKeyPairSync } from 'crypto';

export default {
  name: 'Login',
  data() {
    return {
      username: '',
      password: ''
    }
  },
  methods: {
    register() {
      const { publicKey, privateKey } = generateKeyPairSync('rsa', {
        modulusLength: 4096,
          publicKeyEncoding: {
            type: 'spki',
            format: 'pem'
          },
          privateKeyEncoding: {
            type: 'pkcs8',
            format: 'pem',
            cipher: 'aes-256-cbc',
            passphrase: 'top secret'
          }
      });
      console.log("Public key", publicKey, "Private key", privateKey);
    }
  }
}
</script>
<style scoped>
  body {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 80vh;
    }
    .LoginCard {
      display: flex;
      flex-direction: column;
      width: 500px;
      box-shadow: 0px 0px 13px 0px rgba(161,161,161,1);
      padding: 30px;
    }

    .LoginCard input, button {
      margin-bottom: 10px;
      height: 50px;
      font-size: 18px;
    }

    input {
      padding-left: 10px;
      font-size: 18px;
    }
    button {
      font-weight: bold;
      outline: none;
      cursor: pointer;
      color: #fff;
    }
    .LoginButton {
      background-color: #52c41a;
    }
    
    .RegisterButton {
      background-color: #1890ff;
    }
</style>
