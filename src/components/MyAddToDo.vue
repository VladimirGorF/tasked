<template>
  <section class="add-todo">
    <form v-if="formVisible" class="add-todo__form" @submit.prevent="addTask">
      <button class="close-button" type="button" @click="closeForm">
        <i class="bi bi-x"></i>
      </button>
      <div class="text-input text-input--focus">
        <input v-model="text" class="input" />
      </div>
      <button class="button button--filled">Add task</button>
    </form>
    <button v-else class="add-todo__show-form-button" @click="showForm">
      <i class="bi bi-plus-lg"></i>
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
      if(this.text){
        this.$emit("addTask", {
        id: Date.now(),
        text: this.text,
        completed: false
      });
      }
      // обнуляем текст
      this.text = ''
    },
  },
  emits: {
    addTask: (todo: Todo) => todo,
    // валидация наличия todo
  },
});
</script>
