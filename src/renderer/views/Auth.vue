<template>
  <div class="AuthCard">
      <input name="username" type="text" placeholder="Username" v-model="username">
      <input name="password" type="password" placeholder="Password" v-model="password">
      <button class="LoginButton" @click="login">Login</button>
      <button class="RegisterButton" @click="register">Register</button>
    </div>  
</template>
<script>
import crypto from 'crypto';
import CryptoJS from 'crypto-js';
import path from 'path';
import io from 'socket.io-client';


export default {
  name: 'Auth',
  data() {
    return {
      username: '',
      password: '',
    }
  },
  mounted() {

    this.$socket.on(`login-successful`, user => {
      console.log("Login başarılı");
      console.log(user);
      window.localStorage.setItem('username', user.username);
      window.localStorage.setItem('public-key', user.publicKey);

      this.$router.push('/home')
    })
  },
  methods: {
    login() {
      const hashedPassword = CryptoJS.SHA256(this.password, { outputLength: 256 }).toString(CryptoJS.enc.Hex);
      this.$socket.emit('user-login', {
        username: this.username,
        password: hashedPassword,
      });
    },
    async register() {
      const hashedPassword = CryptoJS.SHA256(this.password, { outputLength: 256 }).toString(CryptoJS.enc.Hex);

      var prime_length = 256;
      var diffHell = crypto.createDiffieHellman(prime_length);

      diffHell.generateKeys('base64');

      this.$socket.emit('new-user', {
        username: this.username,
        password: hashedPassword,
        publicKey: diffHell.getPublicKey('hex'),
        privateKey: diffHell.getPrivateKey('hex'),
      });
      
      // fs.writeFile(`/users/${this.username}.txt`, diffHell.getPublicKey('hex'), function (err) {
      //   if (err) throw err;
      //   console.log('Saved!');
      // });

      // fs.writeFile(`/users/${this.username}/privateKey.txt`, diffHell.getPublicKey('hex'), function (err) {
      //   if (err) throw err;
      //   console.log('Saved!');
      // });

      // fs.writeFile(`/users/${this.username}/password.txt`, hashedPassword, function (err) {
      //   if (err) throw err;
      //   console.log('Saved!');
      // });
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
    .AuthCard {
      display: flex;
      flex-direction: column;
      width: 500px;
      box-shadow: 0px 0px 13px 0px rgba(161,161,161,1);
      padding: 30px;
    }

    .AuthCard input, button {
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