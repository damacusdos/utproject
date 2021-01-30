<template>
  <div id="app">
    <Repolist />
    <Container />
  </div>
</template>

<script>
import Repolist from './components/repolist.vue'
import Container from './components/container.vue'


export default {
  name: 'App',
  components: {
    Repolist,
    Container
  }
}
</script>

<style>

</style>
