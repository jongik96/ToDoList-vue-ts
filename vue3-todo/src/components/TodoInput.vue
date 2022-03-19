<template>
  <div>
    <label for="todo-input">오늘 할 일 : </label>
    <input
      id="todo-input"
      :value="modelValue"
      type="text"
      @input="onInput"
      @keyup.enter="addTodo"
    />
    <button class="btn" @click="addTodo" type="button">추가</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  props: ["modelValue"],
  methods: {
    // TodoInput 컴포넌트에서 입력하지만 입력한 데이터는 App.vue 에서 관리된다.
    onInput(event) {
      this.$emit("update:modelValue", event.target.value);
    },

    // 추가 버튼 이벤트
    addTodo() {
      this.$emit("add");
    },
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
