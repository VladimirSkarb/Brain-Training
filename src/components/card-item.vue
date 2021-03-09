<template>
  <div>
    <div class="card-item"
         v-on:click="$emit('selectCard', card)"
         v-bind:class="{pointer_none: card.select, transform: card.select, match_bg: card.match}">
      <img
        :src="require(`@/assets/img/image${card.title}.png`)"
        :alt="card.title"
        :datatype="card.id">
    </div>
  </div>
</template>

<script>
export default {
  props: ['card'],
};
</script>

<style scoped>
.card-item {
  width: 150px;
  height: 150px;
  border-radius: 5px;
  background-color: #D624D0;
  margin: 5px 5px;
  transform-style: preserve-3d;
  transition: transform 1s;
}
.card-item img {
  height: 100%;
  width: 100%;
  transform: rotateY(
    180deg
  );
}
.transform {
  transform: rotateY(
    180deg
  );
}
.pointer_none {
  pointer-events: none;
}
.match_bg {
  background-color: #FFF;
}
</style>
