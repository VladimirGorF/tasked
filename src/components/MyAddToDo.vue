<template>
  <section class="add-todo">
    <form v-if="formVisible" class="add-todo__form" @submit.prevent="addTask">
      <button class="close-button" type="button" @click="closeForm">
        <i class="bi bi-x"></i>
      </button>
      <div class="text-input text-input--focus">
        <textarea
          v-model="text"
          class="input"
          placeholder="To wash my car in the detailing center"
        ></textarea>
      </div>
      <button id="add" class="button button--filled">Add task</button>
    </form>
    <button v-else class="add-todo__show-form-button" @click="showForm">
      <a href="#add"><i class="bi bi-plus-lg"></i></a>
    </button>
  </section>
</template>

<script lang="ts">
import { Todo } from "@/todo";
import { defineComponent } from "vue";
interface State {
  formVisible: boolean;
  text: string;
}
export default defineComponent({
  data(): State {
    return {
      formVisible: false,
      text: "",
    };
  },
  methods: {
    showForm() {
      this.formVisible = true;
    },
    closeForm() {
      this.formVisible = false;
    },
    addTask() {
      // если текст задачи введен, то эмитим ее в APP
      if (this.text) {
        this.$emit("addTask", {
          id: Date.now(),
          text: this.text,
          completed: false,
        });
      }
      // обнуляем текст
      this.text = "";
      this.closeForm();
    },
  },
  emits: {
    addTask: (todo: Todo) => todo,
    // валидация наличия todo
  },
});
</script>
