<template>
  <li
    class="todo-item"
    :class="{ 'todo-item--done': todo.completed }"
    @click="togglerToDo"
  >
    <div class="todo-item__status">
      <i class="bi bi-check2"></i>
    </div>
    <span class="todo-item__text">{{ todo.text }}</span>
    <!-- клик.стоп остановит всплытие события после срабатывания и не вызовет Тоггл выше -->
    <button class="todo-item__remove-button" @click.stop="removeTask">
      <i class="bi bi-trash3"></i>
    </button>
    <button
      class="todo-item__remove-button"
      @click.stop="showForm"
    >
      <i class="bi bi-pencil"></i>
    </button>
  </li>
  <form
    class="add-todo__form"
    @submit.prevent="changeTask"
    v-if="formVisible"
  >
    <button class="close-button-changeForm" type="button" @click="closeForm">
      <i class="bi bi-x"></i>
    </button>
    <div class="text-input text-input--focus">
      <input v-model="newText" class="input" />
    </div>
    <button class="button button--filled" type="submit">Change</button>
  </form>
</template>

<script lang="ts">
import { Todo } from "@/todo";
import { PropType, defineComponent } from "vue";

interface State {
  newText: string;
  formVisible: boolean;
}
export default defineComponent({
  props: {
    todo: {
      type: Object as PropType<Todo>,
      required: true,
    },
  },
  data(): State {
    return {
      newText: "",
      formVisible: false
    };
  },
  methods: {
    showForm() {
      this.formVisible = true;
    },
    closeForm() {
      this.formVisible = false;
    },
    togglerToDo() {
      this.$emit("togglerToDo", this.todo.id);
    },
    removeTask() {
      this.$emit("removeTask", this.todo.id);
    },
    changeTask() {
      this.$emit("changeTask", this.todo.id, this.newText);
    },
  },
  emits: {
    togglerToDo: (id: number) => Number.isInteger(id),
    removeTask: (id: number) => Number.isInteger(id),
    changeTask: (id: number, newText: string) =>
      Number.isInteger(id) && typeof newText === "string",
  },
});
</script>
