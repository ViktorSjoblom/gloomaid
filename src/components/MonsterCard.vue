<template>
  <div class="monster-card" :class="{ elite: activeMonster.elite }">

    <div v-if="copies.length > 0" class="tab-container">
      <div class="tab" v-for="(_, index) in copies" :key="copies[index].id"
        :class="{ active: copies[index] === activeMonster }" @click="switchTab(copies[index])">
        {{ copies[index].name }}
      </div>
    </div>

    <h3 class="monster-name" @click="toggleExpanded">{{ showNameOnly ? activeMonster.name : 'Collapse' }}</h3>
    <div v-if="!showNameOnly" class="card-details">
      <div class="form-group">
        <!-- <label>Name:</label> -->
        <input v-model="activeMonster.name" type="text" placeholder="Name" />
        <div class="number-buttons">
          <button v-for="num in 6" :key="num" @click="addNumberToName(num)">{{ num }}</button>
        </div>
      </div>
      <div class="form-group">
        <label>Elite:</label>
        <input v-model="activeMonster.elite" type="checkbox" />
      </div>
      <div class="form-group" style="padding-bottom:5px;">
        <!-- <label>Health:</label> -->
        <input v-model="activeMonster.hp" type="number" placeholder="Health" />
      </div>
      <div class="form-group">
        <!-- <label>Shield:</label> -->
        <input v-model="activeMonster.shield" type="number" placeholder="Shield" />
      </div>
      <div class="form-group">
        <label>Poison:</label>
        <input v-model="activeMonster.poison" type="checkbox" />
      </div>
      <div class="form-group">
        <label>Wound:</label>
        <input v-model="activeMonster.wound" type="checkbox" />
      </div>
      <div class="form-group">
        <label>Immobilize:</label>
        <input v-model="activeMonster.immobilize" type="checkbox" />
      </div>
      <div class="form-group">
        <label>Stun:</label>
        <input v-model="activeMonster.stun" type="checkbox" />
      </div>
      <div class="form-group">
        <label>Muddle:</label>
        <input v-model="activeMonster.muddle" type="checkbox" />
      </div>
      <div class="form-group">
        <label>Disarm:</label>
        <input v-model="activeMonster.disarm" type="checkbox" />
      </div>
      <div class="form-group">
        <div class="action-row" style="padding-bottom:5px;">
          <input v-model="damageAmount" type="number" min="0" placeholder="Damage" />
        </div>
        <!-- Change 1 -->
        <div class="action-row">
          <input v-model="pierceAmount" type="number" min="0" placeholder="Pierce" />
        </div>
        <button class="button-style" @click="dealDamage">Deal damage</button>
        <div style="display:flex;flex-direction:column;align-items:center;">
          <button class="button-style copy" @click="copyMonster">Copy</button>
          <div class="buttons">
            <button style="margin-right:25px;" class="button-style remove" @click="removeMonster">Remove monster</button>
            <button class="button-style remove" @click="removeAllCopies">Remove all</button>
          </div>

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
      copies: [],
      activeMonster: {},
    };
  },
  methods: {
    switchTab(copy) {
      this.activeMonster = this.copies.find((c) => c.id === copy.id);
    },
    copyMonster() {
      const copy = { ...this.activeMonster };
      copy.id = Date.now();
      copy.name = this.activeMonster.name; // Set the copied monster's name to the original monster's name
      copy.isActive = false;
      if (!this.copies) {
        this.copies = []; // Initialize the copies array if it's undefined
      }
      this.copies.push(copy);
      if (this.copies.length <= 6) {
        this.activeTab = this.copies.length - 1;
      }
    },


    toggleExpanded() {
      this.showNameOnly = !this.showNameOnly;
    },

    dealDamage() {
  const damage = parseInt(this.damageAmount);
  const playerPierce = parseInt(this.pierceAmount);
  const shieldValue = parseInt(this.activeMonster.shield);
  if (!Number.isNaN(damage)) {
    if (shieldValue > 0 && playerPierce > 0) {
      const effectiveShield = Math.max(0, shieldValue - playerPierce);
      this.activeMonster.hp -= Math.max(0, damage - effectiveShield);
    } else if (shieldValue > 0) {
      this.activeMonster.hp -= Math.max(0, damage - shieldValue);
    } else {
      this.activeMonster.hp -= damage;
    }
    if (this.activeMonster.poison) {
      this.activeMonster.hp -= 1; // Add 1 to the damage if the monster is poisoned
    }

    this.damageAmount = ''; // Set damageAmount to empty string to clear the input field
  }
},

    addNumberToName(num) {
      const existingNumber = this.activeMonster.name.match(/\d+/); // Extract existing number from the name
      if (existingNumber) {
        const updatedName = this.activeMonster.name.replace(existingNumber[0], num); // Replace existing number with the new one
        this.activeMonster.name = updatedName.trim(); // Trim any leading/trailing spaces
      } else {
        this.activeMonster.name += ` ${num}`; // Add the new number if there is no existing number
      }
    },
    removeMonster() {
      if (this.copies.length === 0) {
        // Emit the remove event for the original card
        this.$emit('remove', this.monster.id);
      } else {
        const index = this.copies.findIndex((copy) => copy === this.activeMonster);
        if (index !== -1) {
          this.copies.splice(index, 1);
          if (index <= this.activeTab) {
            this.activeTab--;
          }
          if (this.activeTab >= 0) {
            this.activeMonster = this.copies[this.activeTab];
          } else {
            this.activeMonster = this.monster;
          }
        }
      }
    },
    removeAllCopies() {
      this.copies = [];
      this.activeMonster = this.monster;
      this.$emit('remove', this.monster.id);
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
  position: relative;
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
  width: 140px;
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

.tab-container {
  position: absolute;
  top: 0;
  right: 0;
  flex-direction: column;
  display: flex;
}

.tab {
  padding: 5px 10px;
  background-color: #ccc;
  cursor: pointer;
  border-radius: 5%;
  flex: 1;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  width: 100px;
}

.tab.active {
  background-color: #aaa;
}

.buttons {
  display: flex;
}

@media only screen and (max-width: 600px) {
  .tab-container {
    right: -65px;
  }

  .tab {
    width: 45px;
  }

  .buttons {
    /* flex-direction: column; */
    display: block;
  }

  .buttons button {
    margin-right: 0px !important;
  }
}
</style>
