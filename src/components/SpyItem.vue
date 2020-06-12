<template>
  <div class="spy-item" @click="$emit('click')">
    <h2>
      <slot name="spy-name" />
    </h2>
    <p>
      <slot name="spy-description" />
    </p>
  </div>
</template>

<script>
export default {
  name: "SpyItem",
  props: {
    likes: String
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.spy-item {
  display: inline-block;
  width: 33.3333%;
  border: 1px solid;
  box-sizing: border-box;
  vertical-align: top;
  padding: 20px;
  cursor: pointer;
  transition: all 200ms ease;
  &:hover {
    background: lightblue;
  }
}
</style>
