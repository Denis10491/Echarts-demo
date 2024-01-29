<template>
  <main class="main">
    <Bar />
    <Line />
  </main>
</template>

<script>
import Bar from './components/Bar.vue';
import Line from './components/Line.vue';

export default {
  components: { Bar, Line }
}
</script>

<style scoped>
.main {
  display: flex;
  justify-content: center;
  flex-direction: column;
  width: 80%;
  height: 100%;
}
.main > * {
  margin: 0 0 20px;
}
</style>