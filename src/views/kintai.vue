<template>
  <div>
    <button @click="start_work">出勤</button>
    <button @click="end_work">退勤</button>
    <button @click="datalist">履歴</button>
    <div>
      <table>
        <tr>
          <th>出勤</th>
          <th>退勤</th>
        </tr>
        <tr v-for="(item,index) in worklist" :key="index">
          <td v-if="item.number == 0">{{item.date}}  {{item.time}}</td>
          <td>{{item.date}}  {{item.time}}</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data(){
    return{
      worklist:[],
      username:"",
    }
  },
  methods:{
    start_work(){
      //一時的に保存したデータを取得
      this.username = sessionStorage.getItem('name')
      console.log(this.username)
      //現在の年月日時を取得
      var start_date = new Date().toLocaleDateString()
      var start_time = new Date().toLocaleTimeString()
      console.log(start_date,start_time)

      var box = {
        "id":this.username,
        "date":start_date,
        "time":start_time,
        "type":"出勤",
        "number":0
      }
        
      //データベースにデータを送る
      axios.post("https://3rh22uarg3.execute-api.us-east-1.amazonaws.com/kintaiApi/putitem",box)
      .then(response =>{
        console.log(response)
        alert("出勤時間は" + start_date+start_time + "です")
      })
    },

    end_work(){
      //現在の年月日時を取得
      var end_date = new Date().toLocaleDateString()
      var end_time = new Date().toLocaleTimeString()
      console.log(end_date,end_time)
      //データベースにデータを送る(動的にURLを変更)
      var box1 = {
        "id":this.username,
        "date":end_date,
        "time":end_time,
        "type":"退勤",
        "number":"1"
      }
      axios.post("https://3rh22uarg3.execute-api.us-east-1.amazonaws.com/kintaiApi/putitem",box1)
      .then(response =>{
        console.log(response)
        alert("出勤時間は" + end_date + end_time + "です")
      })

    },
    async datalist(){
      //データベースからデータを取得(動的にURLを変更)
     var box = await axios.get("https://3rh22uarg3.execute-api.us-east-1.amazonaws.com/kintaiApi/getitem")
     this.worklist = box.data
     console.log(this.worklist)
    }
  }
}
</script>