<template>
  <div id="app">
    <h1>{{ title }}</h1>
    <MonsterTracker />
  </div>
</template>

<script>
import MonsterTracker from './components/MonsterTracker.vue';

export default {
  name: 'App',
  components: {
    MonsterTracker,
  },
  data() {
    return {
      title: 'gloomaid',
    };
  },
};
</script>

<style>

@font-face {
  font-family: 'Gloom';
  src: url('./fonts/IMMORTAL.ttf');
}
#app {
  text-align: center;
  padding: 20px;
  font-family: "Roboto", Helvetica, sans-serif;
}

h1 {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 20px;
  font-family: "Gloom", sans-serif;
}
</style>
