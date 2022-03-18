<template>
  <li class="wrapper">
    <span class="item" :class="todoItemClass" @click="toggleItem">{{
      todoItem.title
    }}</span>
    <button class="btn" @click="removeItem">삭제</button>
  </li>
</template>

<script lang="ts">
import Vue, { PropType } from "vue";
import { Todo } from "../App.vue";

export default Vue.extend({
  props: {
    todoItem: Object as PropType<Todo>,
    index: Number,
  },
  computed: {
    // style 조건
    todoItemClass(): string | null {
      return this.todoItem.done ? "complete" : null;
    },
  },
  methods: {
    toggleItem() {
      this.$emit("toggle", this.todoItem, this.index);
    },
    removeItem() {
      this.$emit("remove", this.index);
    },
  },
});
</script>

<style scoped>
.wrapper {
  display: grid;
  grid-template-columns: 6fr 1fr 5fr;
  margin-bottom: 10px;
}
.item {
  cursor: pointer;
  border: 2px solid;
  border-color: bisque;
  border-radius: 10%;
  margin-right: 10px;
}
.complete {
  text-decoration: line-through;
}
.btn {
  float: right;
  background-color: beige;
  border: none;
  border-radius: 10%;
}
</style>
