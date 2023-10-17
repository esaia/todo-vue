<template>
  <div class="wrapper">
    <div class="todo-box">
      <div v-if="todoList.length !== 0" class="header">
        <h2>Todo list</h2>
        <div class="todo-info">
          <p class="info">All Todos: {{ todoList.length }}</p>

          <p class="info">
            completed: {{ todoList.filter((todo) => todo.completed).length }}
          </p>
        </div>
      </div>

      <form @submit.prevent="addNewTodo" class="input">
        <input
          v-model="newTodoText"
          type="text"
          class="text-input"
          id="new-input"
        />
        <button type="submit" class="plus-btn">
          <Plus class="icon" />
        </button>
      </form>

      <TodoItem
        v-for="(todoItem, i) in todoList"
        :key="i"
        :todoItem="todoItem"
        @toogleComplete="toogleComplete"
        @editTodo="editTodo"
        @deleteTodo="deleteTodo"
      />
    </div>
  </div>
</template>

<script setup>
import { computed, onMounted, onUpdated, reactive, ref } from "vue";
import TodoItem from "./TodoItem.vue";
import Plus from "../assets/icons/Plus.vue";
import axios from "axios";

const todoList = ref([]);

const newTodoText = ref("");

const findIndexInTodoList = (todo) => {
  const index = todoList.value.findIndex((todoItem) => todoItem.id === todo.id);
  return index;
};

const addNewTodo = () => {
  if (!newTodoText.value) return;
  const newTodo = { id: Date.now(), title: newTodoText.value, computed: false };
  todoList.value.unshift(newTodo);
  newTodoText.value = "";
};

const toogleComplete = (todo) => {
  const index = findIndexInTodoList(todo);
  if (index !== -1) {
    todoList.value[index] = { ...todo, completed: !todo.completed };
  }
};

const editTodo = ({ todoItem, updatedText }) => {
  const index = findIndexInTodoList(todoItem);
  if (index !== -1) {
    todoList.value[index] = { ...todoItem, title: updatedText };
  }
};

const deleteTodo = (todoItem) => {
  todoList.value = todoList.value.filter((todo) => todo.id !== todoItem.id);
};

onMounted(() => {
  const fetchData = async () => {
    try {
      const { data } = await axios.get(
        "https://jsonplaceholder.typicode.com/todos"
      );
      todoList.value.push(...data);
    } catch (error) {
      console.error(error);
    }
  };

  fetchData();
});
</script>

<style scoped>
.wrapper {
  padding: 20px 6px;
}

.header {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.todo-info {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 5px;
}
.info {
  width: 110px;
}

.todo-box {
  margin: auto;
  max-width: 500px;
  border-radius: 3px;
  padding: 15px;
  background-color: white;
}

.input {
  display: flex;
  gap: 5px;
  justify-content: center;
  align-items: center;
  padding: 10px 0px;
}

.text-input {
  width: 100%;
  outline: none;
  padding: 5px;
}

.plus-btn {
  background-color: transparent;
  border: none;
}
</style>
