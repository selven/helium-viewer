<template>
  <div class="percentage"></div>
</template>

<script>
export default {
  name: "Percentage",
  props: {
    color: String,
  },
};
</script>

<style scoped>
.percentage {
  max-width: 300px;
  width: 100%;
  height: 50px;
  border-radius: 25px;
  background: #000;
}
</style>
