<template>
  <div class="monster-tracker">
    <h1>gloomaid</h1>
  <button @click="addMonster" class="add-card-button">Add Monster</button>
    <div class="card-container">
      <div v-for="(monster, index) in monsters" :key="index" class="card">
          <label>Name:</label>
        <h3 class="card-title">
          <input type="text" v-model="monster.name" class="card-name" />
        </h3>
        <div class="card-info">
          <label>HP:</label>
          <input type="number" v-model="monster.hp" min="0" class="card-input" />
        </div>
        <div class="card-info">
          <label>Shield:</label>
          <input type="number" v-model="monster.shields" min="0" class="card-input" />
        </div>
        <div class="status-options">
          <div class="status-option">
            <label>Poisoned:</label>
            <input type="checkbox" v-model="monster.poisoned" class="status-checkbox" />
          </div>
          <div class="status-option">
            <label>Disarmed:</label>
            <input type="checkbox" v-model="monster.disarmed" class="status-checkbox" />
          </div>
          <div class="status-option">
            <label>Wounded:</label>
            <input type="checkbox" v-model="monster.wounded" class="status-checkbox" />
          </div>
          <div class="status-option">
            <label>Muddled:</label>
            <input type="checkbox" v-model="monster.muddled" class="status-checkbox" />
          </div>
          <div class="status-option">
            <label>Stunned:</label>
            <input type="checkbox" v-model="monster.stunned" class="status-checkbox" />
          </div>
        </div>
        <button @click="removeMonsterCard(index)" class="remove-card-button">Remove Card</button>
      </div>
    </div>
  </div>
</template>



<script>
export default {
  data() {
    return {
      monsters: [],
    };
  },
  methods: {
     // Add a new monster card
    addMonster() {
      this.monsters.push({
        name: '',
        hp: 0,
        shield: 0,
        poisoned: false,
        disarmed: false,
        wounded: false,
        muddled: false,
        stunned: false,
      });

      // Delay the execution of saveMonsterCards using a promise or setTimeout
      Promise.resolve().then(() => {
        this.saveMonsterCards();
      });
    },
    removeMonsterCard(index) {
      this.monsters.splice(index, 1);
    },
   // Save the monster cards to local storage
    saveMonsterCards() {
      // Convert the monsters array to JSON
      const monstersJSON = JSON.stringify(this.monsters);

      // Save the JSON string to local storage
      localStorage.setItem('monsterCards', monstersJSON);
    },
    // Load the monster cards from local storage
    loadMonsterCards() {
      // Retrieve the JSON string from local storage
      const monstersJSON = localStorage.getItem('monsterCards');

      if (monstersJSON) {
        // Parse the JSON string into an array of monsters
        this.monsters = JSON.parse(monstersJSON);
      }
    },
  },
  mounted() {
    // Load the monster cards when the component is mounted
    this.loadMonsterCards();
  },


  watch: {
    // Save the monster cards whenever the monsters array changes
    monsters: {
      handler() {
        this.saveMonsterCards();
      },
      deep: true,
    },
  },
  
};
</script>


<style>
@font-face {
  font-family: "GloomhavenFont";
  src: url("../fonts/IMMORTAL.ttf");
}

.monster-tracker {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  text-align: center;
  font-family: "GloomhavenFont", sans-serif;
}

label {
    font-family: "Roboto", sans-serif;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
}

.card {
  width: calc(25% - 20px);
  margin-bottom: 20px;
  background-color: #f9f9f9;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 20px;
  box-sizing: border-box;
  margin: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.card-title {
  margin-top: 0;
}

.card-info {
  margin-top: 10px;
}

.card-input {
  width: 50px;
  padding: 5px;
  border: 1.5px solid rgb(45, 44, 44);
  border-radius: 5px;
}

.card-name {
  font-size: 18px;
  border: 1.5px solid rgb(45, 44, 44);
  border-radius: 5px;
  background-color: transparent;
  width: 100px;
}

.add-card-button,
.remove-card-button {
  background-color: #313931;
  color: white;
  border: none;
  padding: 8px 16px;
  font-size: 14px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.add-card-button:hover,
.remove-card-button:hover {
  background-color: #8b3f3f;
}

@media (max-width: 600px) {
  .monster-tracker {
    text-align: center;
  }

  .add-card-button {
    margin-top: 20px;
  }

  .card-container {
    flex-direction: column;
  }

  .card {
    width: 100%;
  }
}
</style>
