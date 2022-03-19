<template>
  <li class="wrapper">
    <span class="item">{{ todoItem.title }}</span>
    <button type="button">삭제</button>
  </li>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  props: {
    todoItem: Object,
    index: Number,
  },
});
</script>

<style scoped></style>
