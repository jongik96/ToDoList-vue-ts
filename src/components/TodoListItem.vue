<template>
  <li>
    <span>{{ todoItem }}</span>
    <button @click="removeItem">삭제</button>
  </li>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  props: {
    todoItem: String,
    index: Number,
  },
  methods: {
    removeItem() {
      this.$emit("remove", this.index);
    },
  },
});
</script>

<style scoped></style>
