<template>
  <div class="door-area">
    <div class="door-frame" :class="{selected: selected && !open}">
      <GiftComponent v-if="open && hasGift" />
    </div>
    <div class="door" :class="{open}" @click="selected = !selected">
      <div class="number" :class="{selected}">{{number}}</div>
      <div class="knob" :class="{selected}" @click.stop="verifyWin"></div>
    </div>
  </div>
</template>

<script>
import GiftComponent from './GiftComponent.vue'

export default {
  name: 'DoorComponent',
  components: { GiftComponent },
  props: {
    number: {},
    hasGift: { type: Boolean },
  },
  data: function () {
    return {
      open: false,
      selected: false
    }
  },
  methods: {
    verifyWin() {
      this.open = true;
      if (this.hasGift) {
        setTimeout(() => {
          this.$swal.fire({
            icon: 'success',
            title: 'You won!',
            text: 'Congratulations'
          });
          this.$parent.finishGame();
        }, 600);
      }
    }
  }
}
</script>

<style scoped>
:first-child {
  --door-border: 5px solid brown;
  --selected-border: 5px solid yellow;
}

.door-area {
  position: relative;
  width: 200px;
  height: 310px;
  border-bottom: 10px solid #AAA;
  margin-bottom: 20px;
  font-size: 3rem;

  display: flex;
  justify-content: center;
}

.door-frame {
  position: absolute;
  height: 300px;
  width: 180px;

  border-right: var(--door-border);
  border-top: var(--door-border);
  border-left: var(--door-border);

  display: flex;
  justify-content: center;
  align-items: flex-end;
}

.door {
  position: absolute;
  top: 5px;
  height: 295px;
  width: 172px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: chocolate;
  cursor: pointer;
}

.door .knob {
  height: 20px;
  width: 20px;
  background-color: brown;
  border-radius: 50%;
  align-self: flex-start;
  margin-top: 60px;
  box-shadow: 4px 4px 8px 4px rgba(0, 0, 0, 0.5);
  cursor: help;
}

.door-frame.selected {
  border-right: var(--selected-border);
  border-top: var(--selected-border);
  border-left: var(--selected-border);
}

.door .number.selected {
  color: yellow;
}

.door .knob.selected {
  background-color: yellow;
}

.door.open {
  background-color: #0007;
}

.door.open .knob {
  display: none;
}

.door.open .number {
  display: none;
}
</style>