<template>
  <div class="w-100 d-flex justify-content-center">
    <div class="w-50 todo_list">
      <div
        class="my-2 p-2 d-flex justify-content-between align-items-center todo_item"
        v-for="todo in todoList"
        :key="todo.id"
      >
        <div :class="{ 'text-success': todo.completed }" v-if="!todo.isEdit">
          {{ todo.title }}
        </div>
        <input type="text" v-model="todo.title" v-if="todo.isEdit" />
        <div>
          <button
            v-if="!todo.completed"
            @click="!todo.isEdit ? editTodo(todo) : updateTitleTodo(todo)"
            class="border-0 text-success mx-2"
          >
            {{ !todo.isEdit ? "Edit" : "Save" }}
          </button>
          <button
            v-if="!todo.completed"
            @click="updateDoneTodo(todo)"
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
    editTodo(todo) {
      todo.isEdit = !todo.isEdit;
    },
    updateTitleTodo(todo) {
      axios
        .put(`http://localhost:3000/api/v1/todos/${todo.id}`, {
          title: todo.title,
        })
        .then((res) => {
          todo.title = res.data.title;
          todo.isEdit = !todo.isEdit;
        })
        .catch((e) => {
          console.log("e :>> ", e);
        });
    },
    updateDoneTodo(todo) {
      axios
        .put(`http://localhost:3000/api/v1/todos/${todo.id}`, {
          completed: true,
        })
        .then((res) => {
          todo.completed = res.data.completed;
        })
        .catch((e) => {
          console.log("e :>> ", e);
        });
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
