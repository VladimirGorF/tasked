<template>
  <ul class="todo-list">
    <MyToDoItem
      v-for="doing in todoList"
      :key="doing.id"
      :todo="{ id: doing.id, text: doing.text, completed: doing.completed }"
      @toggler-to-do="togglerToDo"
      @remove-task="removeTask"
      @change-task="changeTask"
    />
  </ul>
</template>

<script lang="ts">
import { PropType, defineComponent } from "vue";
import MyToDoItem from "./MyToDoItem.vue";
import { Todo } from "@/todo";

export default defineComponent({
  components: {
    MyToDoItem,
  },
  props: {
    todoList: {
      type: Array as PropType<Todo[]>,
    },
  },
  methods: {
    togglerToDo(id: number) {
      this.$emit("togglerToDo", id);
    },
    removeTask(id: number) {
      this.$emit("removeTask", id);
    },
    changeTask(id: number, newText:string) {
      this.$emit("changeTask", id, newText);
    },
  },
  emits: {
    togglerToDo: (id:number) => Number.isInteger(id),
    removeTask: (id:number) => Number.isInteger(id),
    changeTask: (id:number,newText:string) => Number.isInteger(id) && typeof newText === 'string'
   },
});
</script>
