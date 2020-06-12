<template>
  <div class="app-progress-bar">
    <span> {{ counter }} </span>
  </div>
</template>

<script>
export default {
  name: "SpyItem",
  props: {
    counter: Number
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss"></style>
