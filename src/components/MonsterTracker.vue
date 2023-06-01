<template>
  <div class="app">
    <div class="monster-tracker">
      <div class="monster-list">
        <div class="button-container">
          <button @click="createNewMonster" class="create-monster-button">Create monster</button>
      <button class="create-monster-button remove" @click="removeAllMonsters">Remove monsters</button>
    </div>
        <ul>
          <li v-for="monster in monsters" :key="monster.id">
            <MonsterCard
              :monster="monster"
              @remove="removeMonster"
              @copy="copyMonster"
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
     copyMonster(copiedMonster) {
    this.monsters.push(copiedMonster); // Add the copied monster to the monsters array
    this.saveMonsters(); // Save the updated monsters array
  },
    saveMonsters() {
      const monsters = JSON.stringify(this.monsters);
      localStorage.setItem('monsters', monsters);
    },
createNewMonster() {
  const monster = {
    id: Date.now(),
    name: '',
    hp: '',
    shield: '',
    poison: false,
    disarm: false,
    wound: false,
    muddle: false,
    stun: false,
    elite: false,
  };
  this.monsters.push(monster);
  this.saveMonsters();
},
    removeMonster(id) {
      this.monsters = this.monsters.filter((monster) => monster.id !== id);
      this.saveMonsters();
    },
    removeAllMonsters() {
      this.monsters = [];
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

.remove {
  background-color: rgb(255, 87, 51);
}

.button-container {
  display: flex;
  justify-content: space-between;
}

@media only screen and (max-width: 600px) {
  .button-container {
  display: flex;
  flex-direction: column;
  width: 60%;
  /* justify-content: space-between; */
}

}
</style>
