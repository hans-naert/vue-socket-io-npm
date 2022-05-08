<template>
  <div id="app">
    <div>{{ message }}</div>
    <ul id="messages">
      <li v-for="message in messages" v-bind:key="message">
        <span>{{ message }}</span>
      </li>
    </ul>

    <form id="form" action="">
      <input v-model="inputText" id="input" autocomplete="off" />
      <button @click.prevent="Send"> Send </button>
    </form>
  </div>
</template>

<script>
import { io } from "socket.io-client";

export default {
  name: "App",
  socket: undefined,
  mounted() {
  this.socket = io('http://localhost:3000/'); 
  this.socket.on("chat message", (msg) => {
    this.messages.push(msg);
    window.scrollTo(0, document.body.scrollHeight);
    }); 
  },
  data() {
    return {
      inputText: "",
      message: "Hello Vue!",
      messages: ["test1", "test2", "test3"],
    };
  },
  methods: {
    Send: function () {
      console.log(this)
      if (this.inputText) {
        this.socket.emit("chat message", this.inputText);
        this.inputText = "";
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
