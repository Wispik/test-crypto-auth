<template>
  <main-component />
</template>

<script>
import MainComponent from './components/MainComponent.vue'

export default {
  components: { MainComponent }
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
#app {
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  gap: 20px;
}
</style>