<script setup>
import TodoList from "./components/TodoList.vue";
</script>

<template>
  <TodoList />
</template>

<style>
.icon {
  width: 18px;
  height: 18px;
  cursor: pointer;
  min-width: 25px;
}

.portal {
  width: 100%;
  height: 100%;
  position: fixed;
  left: 0;
  top: 0;
  background-color: rgba(32, 31, 31, 0.47);
  display: flex;
  justify-content: center;
  align-items: center;
}

.portal-box {
  background-color: rgb(247, 247, 246);
  border-radius: 4px;
  padding: 10px;
  min-width: 200px;
  font-weight: bolder;
}

.button-wrapper {
  display: flex;
  justify-content: end;
  gap: 6px;
  margin-top: 5px;
}

.delete-btn {
  background-color: rgb(232, 62, 62);
  cursor: pointer;
  color: rgb(255, 255, 255);
  border: none;
  border-radius: 4px;
}
</style>
