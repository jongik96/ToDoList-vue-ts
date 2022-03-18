<template>
  <div>
    <h1>Vue Todo With TypeScript</h1>
    <TodoInput
      :item="todoText"
      @input="updateTodoText"
      @add="addTodoItem"
    ></TodoInput>
    <!-- <input type="text" v-model="todoText"> -->
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import TodoInput from "./components/TodoInput.vue";

export default Vue.extend({
  components: { TodoInput },
  data() {
    return {
      todoText: "",
    };
  },
  methods: {
    updateTodoText(value: any) {
      this.todoText = value;
    },
    addTodoItem() {
      const value = this.todoText;
      localStorage.setItem(value, value);
      this.initTodoText();
    },
    initTodoText() {
      this.todoText = "";
    },
  },
});
</script>

<style scoped></style>
