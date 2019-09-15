<template>
  <div id="app">
    <button @click="test">
      测试
    </button>
    {{ content }}
  </div>
</template>

<script>
import * as signalR from "@aspnet/signalr";

export default {
  name: 'app',
  components: {
    //
  },
  data() {
    return {
      content: '测试内容',
    }
  },
  mounted() {
    console.log('signalR: ', signalR);
  },
  methods: {
    test() {
      const self = this;
      const connect = new signalR.HubConnectionBuilder().withUrl('/chathub').build();
      
      connect.start().catch(err => {
        console.error('err:', toString(err))
      });
      connect.on('someFunc', (obj) => {
        console.log('someFunc', obj)
      });
      connect.on('ReceiveMessage', (obj) => {
        console.log('ReceiveMessage: ', obj)
      })
      connect.on('finished', (obj) => {
        connect.stop();
        console.log('finished: ', obj);
      })
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
