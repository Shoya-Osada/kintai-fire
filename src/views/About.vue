<template>
  <div class="about">
    <h1>新規ログイン画面</h1>
    <input type="text" v-model="email" placeholder="メールアドレスを入力">
    <div>
      <input type="password" v-model="password" placeholder="パスワードを入力">
    </div>
    <button @click="login">作成</button>
  </div>
</template>

<script>

import axios from 'axios'
export default {
  data(){
    return{
      email:"",
      password:"",
    }
  },
  methods:{
    async login(){
      console.log("aaaa")
      //通信ライブラリでdataを送る
      var test = await axios.post("https://identitytoolkit.googleapis.com/v1/accounts:signUp?key=" + "AIzaSyAR4HImtY6KRc9Mv0QjHsbWn0FLfl-JEgc",{

        email:this.email,
        password:this.password
      })
      .then(response => {
        console.log(response)
        alert("登録完了。再度ログインしてください。")
        
      }).catch(error =>{
        console.log(test)
        console.log(error)
      });

      this.email = "";
      this.password = "";
      
    }
  }
}
</script>


