<template>
  <div class="about">
    <h1>新規ログイン画面</h1>
    <input type="text" v-model="email" placeholder="メールアドレスを入力">
    <div>
      <input type="password" v-model="password" placeholder="パスワードを入力">
    </div>
    <div>
      <input type="text" v-model="dspname" placeholder="任意のIDを入力してください">
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
      dspname:"",
    }
  },
  methods:{
    async login(){
      //アロー関数の中でthisが使えない（階層が違くなる）のを防ぐ
      var self = this
      console.log("aaaa")
      //通信ライブラリでdataを送る
      var test = await axios.post("https://identitytoolkit.googleapis.com/v1/accounts:signUp?key=" + "AIzaSyAR4HImtY6KRc9Mv0QjHsbWn0FLfl-JEgc",{

        email:this.email,
        password:this.password,
        displayName:this.dspname
      })
      .then(response => {
        console.log(response)
        //帰ってきたデータをuseridにいれる
        self.userid = response.data.displayName
        console.log(self.userid)
        //ブラウザを切った後に、消えるストレージに一時的に保存する
        sessionStorage.setItem('name',self.userid)
        alert("登録完了。再度ログインしてください。")

      }).catch(error =>{
        console.log(test)
        console.log(error)
        alert("入力されていない項目があります。再度打ちなおしてください。")
        //間違っていたら打ち直し
        this.email = "";
        this.password = "";
        this.dspname = "";
      });

      
      
    }
  }
}
</script>


