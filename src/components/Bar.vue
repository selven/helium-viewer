<template>
  <div class="bar" v-bind:style="{ height: percent + '%' }"></div>
</template>

<script>
export default {
  name: "Bar",
  props: {
    max: Number,
    value: Number,
  },
  computed: {
    percent: function () {
      return (this.value / this.max) * 100;
    },
  },
};
</script>

<style scoped>
.bar {
  margin: 0 5px;
  min-height: 10px;
  width: 20px;
  border-radius: 10px;
  float: left;
  margin-top: auto;
  background: #fff;
}
</style>
