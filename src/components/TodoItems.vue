<template>
  <div class="w-100 d-flex justify-content-center">
    <div class="w-50 todo_list">
      <div
        class="my-2 p-2 d-flex justify-content-between align-items-center todo_item"
        v-for="todo in todoList"
        :key="todo.id"
      >
        <div :class="{ 'text-success': todo.completed }">{{ todo.title }}</div>

        <div>
          <button
            v-if="!todo.completed"
            @click="updateTodo(todo)"
            class="border-0 text-success mx-2"
          >
            Done
          </button>
          <button @click="removeTodo(todo.id)" class="border-0 text-danger">
            Remove
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: {
    todoList: Array,
  },
  methods: {
    updateTodo(todo) {
      todo.completed = true;
    },
    removeTodo(id) {
      axios
        .delete(`http://localhost:3000/api/v1/todos/${id}`)
        .then(() => {
          this.$emit("remove-todo", id); // Emit custom event to remove todo item
        })
        .catch((error) => {
          console.log("error :>> ", error);
        });
    },
  },
};
</script>

<style lang="scss">
.todo_item {
  min-height: 40px;
  border-radius: 3px;
  background-color: rgba(86, 61, 124, 0.15);
}
</style>
