<template>
  <div id="app">
    <h1>Monty Hall Problem</h1>
    <form @submit.prevent>
      <div v-if="!started">
        <label for="doorsAmount">How many doors?</label>
        <input type="range" id="doorsAmount" min="3" max="10" v-model.number="doorsAmount" />
        {{ doorsAmount }}
      </div>
      <div v-if="!started">
        <label for="selectedDoor">Which one will be the awarded door</label>
        <select id="selectedDoor" required v-model.number="selectedDoor">
          <option v-for="n in doorsAmount" :key="n">{{ n }}</option>
        </select>
      </div>
      <button type="submit" v-if="!started" @click="showAlert">Begin</button>
      <button v-if="started" @click="started = false">Restart</button>
    </form>
    <div class="doors" v-if="started">
      <div v-for="i in doorsAmount" :key="i">
        <DoorComponent :hasGift="i === selectedDoor" :number="i" v-model="started"/>
      </div>
    </div>
  </div>
</template>

<script>
import DoorComponent from './components/DoorComponent.vue'

export default {
  name: 'App',
  components: { DoorComponent },
  data: function () {
    return {
      started: false,
      doorsAmount: 3,
      selectedDoor: 1
    }
  },
  methods: {
    showAlert() {
      this.$swal.fire({
        title: 'Are you sure?',
        text: "Game is about to begin",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, begin!'
      }).then((result) => {
        if (result.isConfirmed) {
          this.$swal.fire({
            title: 'Good luck',
            toast: true,
            position: 'top',
            timer: 1500,
            showConfirmButton: false,
          });
          this.started = true;
        }
      });
    },
    finishGame() {
      this.started = false;
    }
  }
}

</script>

<style>
* {
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;
}

body {
  color: white;
  background: linear-gradient(to right, rgb(21, 153, 87), rgb(21, 87, 153));
}

#app {
  display: flex;
  flex-direction: column;
  align-items: center;
}

#app h1 {
  border: 1px solid black;
  background-color: #0004;
  padding: 20px;
  margin-bottom: 60px;
}

form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
}

form,
input,
button,
select {
  margin-bottom: 10px;
  font-size: 4.5vw;
}

.doors {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
</style>