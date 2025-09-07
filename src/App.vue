<template>
  <div class="home">
    <Header />
    <ChatBot msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import ChatBot from './components/ChatBot.vue'
import Header from './components/Header.vue'

export default {
  name: 'App',
  components: {
    ChatBot,
    Header
  }
}
</script>