<template>
  <div class="todo">
    <div v-if="!showEditInput" class="todo-items">
      <input
        :id="todoItem.id"
        :checked="todoItem.completed"
        type="checkbox"
        class="checkbox-input"
        @click="$emit('toogleComplete', todoItem)"
      />

      <p
        :class="{ 'item-text': true, completed: todoItem.completed }"
        @dblclick="showEditInputFn"
      >
        {{ todoItem.title }}
      </p>
      <Edit class="icon" @click="showEditInputFn" />
    </div>

    <!-- Else -->
    <form @submit.prevent="submitForm" v-else class="todo-items">
      <input
        ref="inputField"
        class="editInput"
        type="text"
        v-model="editText"
        :id="todoItem.id"
      />

      <Exit class="icon" @click="hideEditInputFn" />
      <button type="submit" class="btn">Edit</button>
    </form>

    <Delete class="icon" @click="showDeleteModal = true" />
  </div>

  <Teleport v-if="showDeleteModal" to="#portal">
    <div class="portal" @click="showDeleteModal = false">
      <div class="portal-box" @click.stop="">
        <p>Are you sure?</p>

        <div class="button-wrapper">
          <button @click="deleteTodo" class="delete-btn">Delete</button>
          <button @click="showDeleteModal = false" class="btn">cancel</button>
        </div>
      </div>
    </div>
  </Teleport>
</template>

<script setup>
import { ref, nextTick, computed } from "vue";
import Delete from "../assets/icons/Delete.vue";
import Edit from "../assets/icons/Edit.vue";
import Exit from "../assets/icons/Exit.vue";

const emit = defineEmits(["editTodo", "toogleComplete", "deleteTodo"]);

const props = defineProps(["todoItem"]);
const showEditInput = ref(false);

const todoItem = computed(() => {
  return props.todoItem;
});

const editText = ref(todoItem.value.title);

const inputField = ref(null);
const showDeleteModal = ref(false);

const showEditInputFn = () => {
  editText.value = todoItem.value.title;
  showEditInput.value = true;
  nextTick(() => {
    inputField.value.focus();
  });
};

const hideEditInputFn = () => {
  showEditInput.value = false;
};

const submitForm = () => {
  emit("editTodo", {
    todoItem: todoItem.value,
    updatedText: editText.value,
  });
  showEditInput.value = false;
};

const deleteTodo = () => {
  emit("deleteTodo", todoItem.value);
  showDeleteModal.value = false;
};
</script>

<style scoped>
.todo {
  display: flex;
  justify-content: start;
  align-items: center;
  width: 100%;
  padding: 4px;
  border-bottom: 1px solid rgb(163, 163, 163);
  margin-bottom: 15px;
}

.todo-items {
  display: flex;
  gap: 15px;
  justify-content: center;
  align-items: center;
  width: 100%;
  padding-right: 10px;
}

.checkbox-input {
  width: fit-content;
}

.item-text {
  width: 100%;
}
.completed {
  text-decoration: line-through;
}
.btn {
  padding: 4px 10px;
  background-color: rgb(6, 7, 7);
  border: none;
  border-radius: 4px;
  cursor: pointer;
  color: white;
}

.editInput {
  width: 100%;
  outline: none;
  border: none;
  margin-left: 27px;
  font-size: 15px;
}
</style>
