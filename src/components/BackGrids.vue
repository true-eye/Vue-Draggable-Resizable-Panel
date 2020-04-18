<template>
  <div class="grids">
    <div class="row" v-for="i in 5" :key="i" :style="{ height: (width + 2 * offset) + 'px' }">
      <div
        class="col"
        v-for="j in 12"
        :key="j"
        :style="{ width: width + 'px', height: width + 'px', margin: offset + 'px' }"
      ></div>
    </div>
    <slot/>
  </div>
</template>

<script>
export default {
  name: "BackGrids",
  props: {
    width: Number,
    offset: Number
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.grids {
  position: absolute;
  top: 0;
  left: 0;
  border: 1px solid gray;
  z-index: -1;
}

.grids .row {
  width: 100%;
}

.grids .row .col {
  width: 20px;
  height: 20px;
  background: rgb(245, 245, 245);
  display: inline-block;
}
</style>
