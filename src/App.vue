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
#app {
  text-align: center;
  padding: 20px;
}

h1 {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 20px;
  font-family: GloomhavenFont,sans-serif;
}
</style>
