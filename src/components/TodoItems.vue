<template lang="">
  <div class="w-100 d-flex justify-content-center">
    <div class="w-50 todo_list">
      <div
        class="my-2 p-2 d-flex justify-content-between align-items-center todo_item"
        v-for="item in todoListCopy"
        :key="item.id"
      >
        <div :class="{ 'text-success': item.completed }">{{ item.title }}</div>

        <div>
          <button
            v-if="!item.completed"
            @click="updateTodo(item)"
            class="border-0 text-success mx-2"
          >
            Done
          </button>
          <button @click="removeTodo(item.id)" class="border-0 text-danger">
            Remove
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    todoList: Array,
  },
  data() {
    return {
      todoListCopy: this.todoList,
    };
  },
  methods: {
    updateTodo(todo) {
      todo.completed = true;
      this.saveTodo();
    },
    removeTodo(id) {
      this.todoListCopy.splice(id, 1);
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
.todo_item {
  min-height: 40px;
  border-radius: 3px;
  background-color: rgba(86, 61, 124, 0.15);
}
</style>
