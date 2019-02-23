<template>
  <div id="app">
    <div id="nav">
      <h1><span class="top-text">Sacrée</span> Tirelire</h1>
    </div>
    <Home/>
  </div>
</template>

<script>
import Home from './views/Home.vue'

export default {
  name: 'app',
  components: {
    Home
  }
}
</script>
