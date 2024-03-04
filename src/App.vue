<template>
  <div class="appBox">
    <div class="headerBox">
      <MyHeader />
      <MyFooter :stats="getStats" />
    </div>
    <MyFilter :active-filter="activeFilter" @set-filter="setFilter" />
    <main class="app-main">
      <MyToDoList
        :todoList="todoListResult"
        @toggler-to-do="togglerToDo"
        @remove-task="removeTask"
        @change-task="changeTask"
      />
    </main>
  </div>
  <MyAddToDo @add-task="addTask" />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import MyHeader from "./components/MyHeader.vue";
import MyFilter from "./components/MyFilter.vue";
import MyToDoList from "./components/MyToDoList.vue";
import MyAddToDo from "./components/MyAddToDo.vue";
import MyFooter, { Stats } from "./components/MyFooter.vue";
import { Todo } from "./todo";
import { Filter } from "./filter";

interface State {
  todoList: Todo[];
  activeFilter: Filter;
}

export default defineComponent({
  components: {
    MyHeader,
    MyFilter,
    MyToDoList,
    MyAddToDo,
    MyFooter,
  },
  data(): State {
    return {
      todoList: [
        { id: 0, text: "Deploy all projects into web", completed: true },
        { id: 1, text: "Show the basics of Vue3", completed: false },
        { id: 2, text: "To buy a new chinise car", completed: true },
        { id: 3, text: "Deploy all projects into web", completed: false },
        { id: 4, text: "Show the basics of Vue3", completed: true },
        { id: 5, text: "To buy a new chinise car", completed: false },
        { id: 6, text: "Deploy all projects into web", completed: true },
        { id: 7, text: "Show the basics of Vue3", completed: false },
        { id: 8, text: "To buy a new chinise car", completed: true },
        { id: 9, text: "Deploy all projects into web", completed: false },
        { id: 10, text: "Show the basics of Vue3", completed: true },
        { id: 11, text: "To buy a new chinise car", completed: false },
      ],
      activeFilter: "All",
    };
  },
  created() {
    this.getStorageData();
  },
  methods: {
    getStorageData() {
      if (localStorage.getItem("tasksGorbunov")) {
        // если данные есть, то получаем их из хранилища
        this.todoList = JSON.parse(localStorage.getItem("tasksGorbunov")!);
      } else {
        this.setStorageData();
      }
    },
    setStorageData() {
      // запись изменений в локал storage
      const dataToSave = JSON.stringify(this.todoList);
      localStorage.setItem("tasksGorbunov", dataToSave);
    },
    addTask(todo: Todo) {
      //получаем данные из хранилища если они есть
      this.getStorageData();
      // записываем новые данные в массив
      this.todoList.push(todo);
      // и запишем изменения в LocalStorage
      this.setStorageData();
    },

    togglerToDo(id: number) {
      const targetToDo = this.todoList.find((todo: Todo) => todo.id === id);
      if (targetToDo) {
        targetToDo.completed = !targetToDo.completed;
        // и запишем изменения в LocalStorage
        this.setStorageData();
      }
    },
    removeTask(id: number) {
      // отфильтруем массив чтобы в нем не было кликнутой задачи
      this.todoList = this.todoList.filter((todo: Todo) => todo.id !== id);
      // и запишем изменения в LocalStorage
      this.setStorageData();
    },
    changeTask(id: number, newText: string) {
      // найти нужную статью и изменить ее
      const targetTask = this.todoList.find((todo: Todo) => todo.id === id);
      targetTask!.text = newText;
      // и запишем изменения в ЛокалStorage
      this.setStorageData();
    },
    setFilter(filter: Filter) {
      this.activeFilter = filter;
    },
  },
  computed: {
    todoListResult(): Todo[] {
      // В зависимости от того, выполнена ли задача и какой тоггл активирован, будем возвращать соответствующий массив
      if (this.activeFilter === "Active") {
        return this.activeTasks;
      } else if (this.activeFilter === "Done") {
        return this.doneTasks;
      } else return this.todoList;
    },
    getStats(): Stats {
      return {
        activeTasks: this.activeTasks.length,
        doneTasks: this.doneTasks.length,
      };
    },
    activeTasks(): Todo[] {
      return this.todoList.filter((todo) => !todo.completed);
    },
    doneTasks(): Todo[] {
      return this.todoList.filter((todo) => todo.completed);
    },
  },
});
</script>
