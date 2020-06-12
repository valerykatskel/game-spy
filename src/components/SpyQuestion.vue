<template>
  <div class="spy-question">
    <h2>{{ question.title }}</h2>
    <p>{{ question.text }}</p>
    <ul>
      <li v-for="answer in question.answers" :key="answer.text">
        <label>
          <input
            name="answer.text"
            type="radio"
            :value="answer.text"
            @click="$emit('answer', [question.id, answer.id])"
          />
          <span> {{ answer.text }} </span>
        </label>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "SpyQuestion",
  props: {
    question: Object
  },
  methods: {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.spy-question {
  ul {
    list-style-type: none;
    margin: 0;
    padding: 0;

    li {
      label {
        cursor: pointer;
      }
    }
  }
}
</style>
