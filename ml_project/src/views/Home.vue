<template>
  <div class="container">
    <input type="checkbox" id="click">
    <label for="click">
      <i class="fab fa-facebook-messenger"></i>
      <i class="fas fa-times"></i>
    </label>
    <div class="wrapper">
         <div class="head-text">
            Let's chat? - Chatbot
         </div>
         <div class="chat-box">
            <form>
              <div v-for="chat in APIResult" :key="chat.id">
                <div class="chatbox chatbox-color">
                  <img src="../assets/user.jpg" alt="Avatar" class="right" style="width:100%;">
                  <p>{{ chat.user }}</p>
                  <span class="time-left">{{ chat.time }}</span>
                </div>
                <div class="chatbox">
                  <img src="../assets/chatbot.jpg" alt="Avatar" style="width:100%;">
                  <p>{{ chat.chatbot }}</p>
                  <span class="time-right">{{ chat.time }}</span>
                </div>
              </div>
            </form>
            <div class="field">
                <input type="text" placeholder="Chat with bot......." v-model.trim="InputData.text" required>
            </div>
            <div class="field">
                <button type="submit" @click="predict"><i class="fa fa-paper-plane-o" style="font-size:20px; color: white; margin-right:10px"></i>Send</button>
            </div>
         </div>
    </div>
  </div>
</template>

<script>
import { getAPI } from "@/axios";

export default {
  name: "Home",
  data() {
    return {
      InputData: {
        text: "",
      },
      APIResult: [],
    }
  },
  methods: {
    predict() {
      getAPI
        .get("/chatting", {
          params: {
            msg: this.InputData.text,
          }
        })
        .then(response => {
          console.log("Recieved data successfully");
          var today = new Date();
          var date = today.getFullYear()+'-'+(today.getMonth()+1)+'-'+today.getDate();
          var time = today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
          var dateTime = date+' '+time;
          this.APIResult = [...this.APIResult, {id: Math. floor(Math. random() * 100), user: this.InputData.text, chatbot: response.data, time: dateTime}];
          console.log(response.data);
        })
        .catch(err => {
          console.log(err);
        });
    }
  }
};
</script>

<style>
body
{
  background: -webkit-linear-gradient(left, #a445b2, yellow);
  height: 55px;
  width: 55px;
}
</style>
