<template>
  <div>
    <header>
      <h1>Vue Todo With TypeScript</h1>
    </header>
    <main>
      <TodoInput
        :item="todoText"
        @input="updateTodoText"
        @add="addTodoItem"
      ></TodoInput>
      <!-- <input type="text" v-model="todoText"> -->
      <div>
        <ul>
          <TodoListItem
            v-for="(todoItem, index) in todoItems"
            :key="index"
            :todoItem="todoItem"
          ></TodoListItem>
        </ul>
      </div>
    </main>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import TodoInput from "./components/TodoInput.vue";
import TodoListItem from "./components/TodoListItem.vue";

const STORAGE_KEY = "vue-todo-ts-v1";
const storage = {
  // localStorage 에 저장할 데이터
  save(todoItems: any[]) {
    const parsed = JSON.stringify(todoItems);
    localStorage.setItem(STORAGE_KEY, parsed);
  },

  // 조회 api
  fetch() {
    const todoItems = localStorage.getItem(STORAGE_KEY) || "[]";
    // 배열 문자열을 Object 형으로 변형
    const result = JSON.parse(todoItems);
    return result;
  },
};

export default Vue.extend({
  components: { TodoInput, TodoListItem },
  data() {
    return {
      todoText: "",
      todoItems: [],
    };
  },
  methods: {
    updateTodoText(value: string) {
      this.todoText = value;
    },
    addTodoItem() {
      const value = this.todoText;
      this.todoItems.push(value);
      storage.save(this.todoItems);
      // localStorage.setItem(value, value);
      this.initTodoText();
    },
    initTodoText() {
      this.todoText = "";
    },

    // storage 에 할 일 담기
    fetchTodoItems() {
      this.todoItems = storage.fetch();
    },
  },
  created() {
    this.fetchTodoItems();
  },
});
</script>

<style scoped></style>
