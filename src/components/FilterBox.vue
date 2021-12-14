<template>
<div>
  <div @click="selectFilter" :class="filters + ' filter-item'" :style="`background-image:url(${tempImgUrl})`">
        <slot></slot>
<!--    <button @click="fire">button</button>-->
  </div>
</div>
</template>

<script>
export default {
  name: "FilterBox",
  methods: {
    selectFilter() {
      this.emitter.emit('applyFilter', this.filters);
    }
  },
  props : {
    tempImgUrl: String,
    filters: String,
  }
}
</script>

<style scoped>
.filter-item {
  width: 100px;
  height: 100px;
  margin: 10px 10px 10px auto;
  padding: 8px;
  display: inline-block;
  color : white;
  background-size: cover;
  background-position : center;
}
</style>