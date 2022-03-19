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
            @remove="removeTodoItem"
            @toggle="toggleTodoItem"
          ></TodoListItem>
        </ul>
      </div>
      <div>
        <button class="btn" @click="removeAll" type="button">전체 삭제</button>
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
    // 할 일 목록 전체 삭제
    removeAll() {
      this.todoItems = [];
      storage.save(this.todoItems);
    },

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
      this.todoItems = storage.fetch().sort((a, b) => {
        if (a.title < b.title) {
          return -1;
        }
        if (a.title < b.title) {
          return 1;
        }
        return 0;
      });
    },

    // 할 일 삭제
    removeTodoItem(index: number) {
      this.todoItems.splice(index, 1);
      storage.save(this.todoItems);
    },

    // 할 일 상태 수정하기
    toggleTodoItem(todoItem: Todo, index: number) {
      this.todoItems.splice(index, 1, {
        ...todoItem,
        // 기존의 상태와 반대로
        done: !todoItem.done,
      });
      storage.save(this.todoItems);
    },
  },

  created() {
    this.fetchTodoItems();
  },
});
</script>

<style scoped>
.btn {
  background-color: beige;
  border: none;
  border-radius: 10%;
  cursor: pointer;
}
</style>
