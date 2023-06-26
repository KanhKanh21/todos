<template>
  <h2 class="font-weight-bold todo_title">Todo App</h2>
  <div class="w-100 d-flex justify-content-center">
    <form class="w-50 d-flex flex-column todo_form" @submit.prevent="addTodo">
      <label class="text-left">New Todo</label>
      <input type="text" v-model="todoName" />
      <button class="my-4 border-0 button_add" type="submit">Add Todo</button>
    </form>
  </div>
  <todo-items :todoList="todoList" />
</template>

<script>
import TodoItems from "./TodoItems.vue";
import axios from "axios";

export default {
  components: { TodoItems },
  name: "todo-list",
  data() {
    return {
      todoName: "",
      todoList: [],
    };
  },

  mounted() {
    this.fetchTodos();
  },

  methods: {
    fetchTodos() {
      axios
        .get("http://localhost:3000/api/v1/todos")
        .then((response) => {
          this.todoList = response.data;
        })
        .catch((error) => {
          console.error(error);
        });
    },

    addTodo() {
      if (this.todoName) {
        this.todoList.push({
          title: this.todoName,
          completed: false,
        });
        this.todoName = "";
      }
      this.saveTodo();
    },

    saveTodo() {
      const storageData = JSON.stringify(this.todoList);
      localStorage.setItem("todoList", storageData);
    },
  },
};
</script>

<style lang="scss">
.todo_form {
  flex-direction: column;
  justify-content: center;
}
.button_add {
  background-color: #563d7c;
  height: 35px;
  color: #fff;
  border-radius: 3px;
}
</style>
