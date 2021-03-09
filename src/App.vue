<template>
  <h1 class="title">Brain Training</h1>
  <CardList
    v-bind:cards="cards"
  />
  <arr-cards/>
</template>

<script>
import CardList from './components/card-list.vue';
import arrCards from './components/mock.vue';

export default {
  name: 'App',
  data() {
    return {
      cards: arrCards,
    };
  },
  components: {
    CardList,
    arrCards,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.title {
  font-size: 30px;
  font-weight: 700;
  color: #7B52B3;
}
</style>
