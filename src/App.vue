<template>
  <div class="home">
    <Header />
    <ChatBot msg="Welcome to Hogwarts" />
    <Footer />
  </div>
</template>

<script>
import ChatBot from './components/ChatBot.vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue';

export default {
  name: 'App',
  components: {
    ChatBot,
    Header, 
    Footer
  }
}
</script>