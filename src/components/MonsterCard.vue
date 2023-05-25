<template>
  <div class="monster-card" :class="{ elite: monster.elite }">
    <h3 class="monster-name" @click="toggleExpanded">{{ showNameOnly ? monster.name : 'Collapse' }}</h3>
    <div v-if="!showNameOnly" class="card-details">
      <div class="form-group">
        <label>Name:</label>
        <input v-model="monster.name" type="text" />
         <div class="number-buttons">
    <button v-for="num in 6" :key="num" @click="addNumberToName(num)">{{ num }}</button>
  </div>
      </div>
        <div class="form-group">
        <label>Elite:</label>
        <input v-model="monster.elite" type="checkbox" />
      </div>
      <div class="form-group">
        <label>Health:</label>
        <input v-model="monster.hp" type="number" placeholder="Health"/>
      </div>
      <div class="form-group">
        <label>Shield:</label>
        <input v-model="monster.shield" type="number" placeholder="Shield" />
      </div>
      <div class="form-group">
        <label>Poison:</label>
        <input v-model="monster.poison" type="checkbox" />
      </div>
      <div class="form-group">
        <label>Wound:</label>
        <input v-model="monster.wound" type="checkbox" />
      </div>
      <div class="form-group">
        <label>Immobilize:</label>
        <input v-model="monster.immobilize" type="checkbox" />
      </div>
      <div class="form-group">
        <label>Stun:</label>
        <input v-model="monster.stun" type="checkbox" />
      </div>
      <div class="form-group">
        <label>Muddle:</label>
        <input v-model="monster.muddle" type="checkbox" />
      </div>
      <div class="form-group">
        <label>Disarm:</label>
        <input v-model="monster.disarm" type="checkbox" />
      </div>
      <div class="form-group">
    <div class="action-row">
      <input v-model="damageAmount" type="number" min="0" placeholder="Damage" />
      <button class="button-style" @click="dealDamage">Deal Damage</button>
    </div>
    <div style="display:flex;flex-direction:column;">
    <button class="button-style copy" @click="copyMonster">Copy</button>
    <button class="button-style remove" @click="removeMonster">Remove Monster</button>
    </div>
  </div>
    </div>
    </div>
</template>

<script>
export default {
  props: {
    monster: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      showNameOnly: false,
      damageAmount: '',
    };
  },
  methods: {
    copyMonster() {
    const copiedMonster = { ...this.monster }; // Create a shallow copy of the monster object
    copiedMonster.id = Date.now(); // Generate a new ID for the copied monster
    this.$emit('copy', copiedMonster); // Emit a 'copy' event with the copied monster as the payload
  },
    toggleExpanded() {
      this.showNameOnly = !this.showNameOnly;
    },
    dealDamage() {
    const damage = parseInt(this.damageAmount);
    if (!isNaN(damage)) {
      if (this.monster.poison) {
        this.monster.hp -= damage + 1; // Add 1 to the damage if the monster is poisoned
      } else {
        this.monster.hp -= damage;
      }
      this.damageAmount = 0;
    }
  },
    dealPierceDamage() {
      // Logic for dealing pierce damage to the monster
    },
    addNumberToName(num) {
      const existingNumber = this.monster.name.match(/\d+/); // Extract existing number from the name
      if (existingNumber) {
        const updatedName = this.monster.name.replace(existingNumber[0], num); // Replace existing number with the new one
        this.monster.name = updatedName.trim(); // Trim any leading/trailing spaces
      } else {
        this.monster.name += ` ${num}`; // Add the new number if there is no existing number
      }
    },
     removeMonster() {
      this.$emit('remove', this.monster.id);
    },
  },
  watch: {
    monsters: {
      deep: true,
      handler() {
      },
    },
  },
};
</script>


<style scoped>
.monster-card {
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  margin-bottom: 10px;
}

.monster-name {
  cursor: pointer;
  margin: 0;
  margin-top: 2.5px;
  margin-bottom: 2.5px;
}

.card-details {
  margin-top: 10px;
}

.elite {
  background-color: rgb(255, 170, 51);
}

.elite input {
    background-color: #313931;
    color: white;
}

.elite .number-buttons button {
    background-color: #313931;
    color: white;
}

.name-row {
  display: flex;
  align-items: center;
  justify-content: center;
}

.number-buttons {
  display: flex;
  margin-left: 10px;
  justify-content: center;
  padding-top: 10px;
  padding-bottom: 10px;
}

.number-buttons button {
  margin-right: 10px;
  border-radius: 15px;
  border: 1px solid black;
}

.button-style {
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

.remove {
    background-color: rgb(255, 87, 51);
}

.button-style:disabled {
  pointer-events: none;
}

.button-style:hover {
  color: #fff;
  background-color: #1A1A1A;
  box-shadow: rgba(0, 0, 0, 0.25) 0 8px 15px;
  transform: translateY(-2px);
}

.button-style:active {
  box-shadow: none;
  transform: translateY(0);
}
</style>