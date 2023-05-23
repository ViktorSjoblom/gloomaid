<template>
  <div class="monster-card" :class="{ elite: monster.elite }">
    <h3 class="monster-name" @click="toggleExpanded">{{ showNameOnly ? monster.name : 'Monster' }}</h3>
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
        <label>HP:</label>
        <input v-model="monster.hp" type="number" />
      </div>
      <div class="form-group">
        <label>Shield:</label>
        <input v-model="monster.shield" type="number" />
      </div>
      <div class="form-group">
        <label>Poison:</label>
        <input v-model="monster.poison" type="checkbox" />
      </div>
      <div class="form-group">
        <label>Wound:</label>
        <input v-model="monster.poison" type="checkbox" />
      </div>
      <div class="form-group">
        <label>Stun:</label>
        <input v-model="monster.poison" type="checkbox" />
      </div>
      <div class="form-group">
        <label>Muddle:</label>
        <input v-model="monster.poison" type="checkbox" />
      </div>
      <div class="form-group">
        <label>Disarm:</label>
        <input v-model="monster.poison" type="checkbox" />
      </div>
      <div class="form-group">        <div>
<label v-for="(status, index) in monster.statuses" :key="index">
  {{ status }}
  <input v-model="monster.statuses[index]" type="text" />
</label>
        </div>
      </div>
      <div class="form-group">
    <div class="action-row">
      <input v-model="damageAmount" type="number" min="0" placeholder="Damage amount" />
      <button @click="dealDamage">Deal Damage</button>
    </div>
    <button @click="removeMonster">Remove Monster</button>
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
      damageAmount: 0,
    };
  },
  methods: {
    toggleExpanded() {
      this.showNameOnly = !this.showNameOnly;
    },
    dealDamage() {
      const damage = parseInt(this.damageAmount);
      if (!isNaN(damage)) {
        this.monster.hp -= damage;
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
}

.card-details {
  margin-top: 10px;
}

.elite {
  background-color: yellow;
}

.name-row {
  display: flex;
  align-items: center;
}

.number-buttons {
  display: flex;
  margin-left: 10px;
}

.number-buttons button {
  margin-right: 5px;
}
</style>
