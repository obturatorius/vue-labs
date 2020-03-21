<template>
  <div class="todo-list">
    <h1>Todo List</h1>
    <TodoItem v-for="todo in groceryList" :todo="todo" :key="todo.id" />
  </div>
</template>

<script>
import TodoItem from "@/components/TodoItem";

export default {
  name: "TodoList",
  components: {
    TodoItem
  },
  data() {
    return {
      groceryList: [
        { id: 1, text: "Buy apples" },
        { id: 2, text: "Buy pears" },
        { id: 3, text: "What do you humans need" }
      ]
    };
  }
};
</script>
