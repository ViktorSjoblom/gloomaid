<template>
  <div class="app">
    <div class="monster-tracker">
      <div class="monster-list">
         <button @click="createNewMonster" class="create-monster-button">Create monster</button>
        <ul>
          <li v-for="monster in monsters" :key="monster.id">
            <MonsterCard
              :monster="monster"
              @remove="removeMonster"
            ></MonsterCard>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import MonsterCard from './MonsterCard.vue';

export default {
  components: {
    MonsterCard,
  },
  data() {
    return {
      monsters: [],
      selectedMonster: null,
    };
  },
  methods: {
    saveMonsters() {
      const monsters = JSON.stringify(this.monsters);
      localStorage.setItem('monsters', monsters);
    },
    createNewMonster() {
      const monster = {
        id: Date.now(),
        name: 'Name',
        hp: '',
        shields: '',
        poisoned: false,
        disarmed: false,
        wounded: false,
        muddled: false,
        stunned: false,
        elite: false,
      };
      this.monsters.push(monster);
      this.saveMonsters();
    },
    removeMonster(id) {
      this.monsters = this.monsters.filter((monster) => monster.id !== id);
      this.saveMonsters();
    },
    loadMonsters() {
      const savedMonsters = localStorage.getItem('monsters');
      if (savedMonsters) {
        this.monsters = JSON.parse(savedMonsters);
      }
    },
  },
  created() {
    this.loadMonsters();
  },
};
</script>

<style scoped>
.app {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.app-title {
  text-align: center;
  font-size: 24px;
  margin-bottom: 20px;
}

.monster-tracker {
  display: flex;
}

.monster-list {
  flex: 1;
}

.monster-list h2 {
  margin-bottom: 10px;
}

.create-monster-button {
  background-color: #313931;
  color: white;
  border: none;
  padding: 8px 16px;
  font-size: 14px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  border-radius: 5px;
  margin-top: 10px;
}

.create-monster-button:hover {
  background-color: #484f4c;
}

.monster-details {
  flex: 2;
  margin-left: 20px;
}

.monster-details h2 {
  margin-bottom: 10px;
}

.selected-monster-card {
  margin-top: 10px;
}

.no-monster-selected {
  text-align: center;
  font-size: 18px;
  color: gray;
  margin-top: 50px;
}

ul {
  list-style: none;
  padding: 0;
}

</style>
