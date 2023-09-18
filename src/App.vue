<template>
  <div id="app">
    <Header />
    <div class="content">
      <Content />
    </div>
    <Footer />
  </div>
</template>

<script>
import Header from './components/Header.vue';
import Content from './components/Content.vue';
import Footer from './components/Footer.vue';

export default {
  components: {
    Header,
    Content,
    Footer
  }
}
</script>

<style>
#app {
  display: flex;
  flex-direction: column;
  /* min-height: 100vh;  */
}

.content {
  /* flex-grow: 1;  */
}
</style>
