<template>
  <div class="wrapper">
    <div class="info">
      <div class="bg-info">Time: {{ minutes }}:{{ seconds }}</div>
      <div class="bg-info">{{ counter }} moves</div>
      <button
        class="reset-btn bg-info"
        v-bind:class="{hide_reset: !hide}"
        v-on:click="resetGame"
      >Reset
      </button>
    </div>
    <div class="card-list">
      <div
        class="modal"
        v-bind:class="{hide: modalHide}">
        <p>You Win</p>
        <button
          v-on:click="resetGame"
        >Play Again
        </button>
      </div>
      <button
        class="start-btn"
        v-on:click="startGame"
        v-bind:class="{hide: hide}"
      >Start
      </button>
      <CardItem
        class="pointerEventsNone"
        v-for="card of arrOfCards" :key="card.id"
        v-bind:card="card"
        v-bind:class="{pointerEventsAuto: hide}"
        v-on:selectCard="onSelect"
      />
    </div>
  </div>
</template>

<script>
import CardItem from './card-item.vue';

export default {
  name: 'CardList',
  props: ['cards'],
  data() {
    return {
      arrOfCards: this.cards,
      counter: 0,
      node: [],
      timerId: 0,
      time: 1,
      minutes: 0,
      seconds: 0,
      hide: false,
      modalHide: true,
    };
  },
  mounted() {
    this.randomCards();
  },
  methods: {
    randomCards() {
      return this.arrOfCards.sort(() => 0.5 - Math.random());
    },
    startGame() {
      setTimeout(this.timer, 1000);
      this.hide = true;
      this.modalHide = true;
    },
    stopGame() {
      clearInterval(this.timerId);
    },
    resetGame() {
      this.stopGame();
      this.time = 0;
      this.randomCards();
      this.startGame();
      this.counter = 0;
      this.node.length = 0;
      for (let i = 0; i < this.arrOfCards.length; i += 1) {
        this.arrOfCards[i].select = false;
        this.arrOfCards[i].match = false;
      }
    },
    timer() {
      this.timerId = setTimeout(() => {
        this.minutes = Math.floor(this.time / 60);
        this.seconds = this.time % 60;
        this.time += 1;
        this.timer();
      }, 1000);
    },
    selectCards() {
      for (let i = 0; i < this.node.length; i += 1) {
        this.node[i].select = false;
      }
      this.node.length = 0;
    },
    matchCards() {
      for (let i = 0; i < 2; i += 1) {
        this.node[i].match = true;
      }
      this.node.length = 0;
    },
    onSelect(e) {
      this.node.push(e);
      if (this.node.length <= 2 && this.node[0] !== this.node[1]) {
        for (let i = 0; i < this.node.length; i += 1) {
          this.node[i].select = true;
        }
      }
      if (this.node.length === 2 && this.node[0].title === this.node[1].title) {
        setTimeout(this.matchCards, 1000);
        this.counter += 1;
        const allCardMatch = this.arrOfCards.every((card) => card.select === true);
        if (allCardMatch) {
          this.stopGame();
          setTimeout(() => { this.modalHide = false; }, 1000);
        }
      } else if (this.node.length === 2 && this.node[0] !== this.node[1]) {
        setTimeout(this.selectCards, 2000);
        this.counter += 1;
      }
    },
  },
  components: {
    CardItem,
  },
};
</script>

<style scoped>
.wrapper {
  width: 640px;
  margin: 0 auto;
}
.info {
  padding: 5px;
  display: flex;
  justify-content: space-between;
}
.card-list {
  width: 640px;
  display: flex;
  flex-wrap: wrap;
  margin: 0 auto;
  position: relative;
}
.pointerEventsNone {
  pointer-events: none;
}
.pointerEventsAuto {
  pointer-events: auto;
}
.start-btn {
  box-sizing: border-box;
  background-color: #7B52B3;
  font-weight: 700;
  color: #FFF;
  position: absolute;
  text-align: center;
  vertical-align: middle;
  border-radius: 8px;
  border: 2px solid #FFF;
  font-size: 25px;
  width: 28%;
  height: 28%;
  top: 35%;
  left: 36%;
  cursor: pointer;
  z-index: 1;
}
.hide {
  display: none;
}
.hide_reset {
  opacity: 0;
}
.reset-btn {
  cursor: pointer;
}
.bg-info {
  box-sizing: border-box;
  width: 100px;
  height: 30px;
  background-color: #7B52B3;
  border-radius: 5px;
  color: #FFF;
  padding: 5px 0 0;
  font-size: 18px;
  font-weight: 700;
  border: none;
}
.modal {
  position: absolute;
  width: 500px;
  height: 200px;
  font-size: 50px;
  font-weight: 700;
  background-color: #7B52B3;
  z-index: 1;
  top: 215px;
  left: 80px;
  border: 2px solid #FFF;
  border-radius: 5px;
  color: #FFF;
  text-align: center;
}
.modal p {
  margin: 20px;
}
.modal button {
  background-color: transparent;
  width: 200px;
  height: 80px;
  color: #FFF;
  border: none;
  font-size: 25px;
  font-weight: 600;
  cursor: pointer;
}
.modal button:hover {
  color: #F58FF1;
}
</style>
