<template>
  <div>
    <header>
      <h1>Vue3 Todo With TypeScript</h1>
    </header>
    <main>
      <TodoInput v-model="todoText" @add="addTodoItem"></TodoInput>
      <div>
        <ul>
          <TodoListItem
            v-for="(todoItem, index) in todoItems"
            :key="index"
            :index="index"
            :todoItem="todoItem"
          ></TodoListItem>
        </ul>
      </div>
    </main>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import TodoListItem from "./components/TodoListItem.vue";
import TodoInput from "./components/TodoInput.vue";

const STORAGE_KEY = "vue3-todo";
const storage = {
  save(todoItems: Todo[]) {
    const parsed = JSON.stringify(todoItems);
    localStorage.setItem(STORAGE_KEY, parsed);
  },

  // 조회 api
  fetch(): Todo[] {
    const todoItems = localStorage.getItem(STORAGE_KEY) || "[]";
    // 배열 문자열을 Object 형으로 변형
    const result = JSON.parse(todoItems);
    return result;
  },
};

export interface Todo {
  title: string;
  done: boolean;
}

export default defineComponent({
  components: { TodoInput, TodoListItem },
  data() {
    return {
      todoText: "" as string,
      todoItems: [] as Todo[],
    };
  },
  methods: {
    // 할 일 추가
    addTodoItem() {
      const value = this.todoText;
      // console.log("add");
      const todo: Todo = {
        title: value,
        done: false,
      };
      this.todoItems.push(todo);
      storage.save(this.todoItems);
      this.initTodoText();
    },

    // 입력창 초기화
    initTodoText() {
      this.todoText = "";
    },

    // 할 일 목록 조회
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
