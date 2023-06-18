<script setup>

import { uid } from 'uid';
import { ref, watch } from 'vue';
import { Icon } from '@iconify/vue';
import TodoItem from '../components/TodoItem.vue';
import TodoCreator from '../components/TodoCreator.vue';
import { computed } from '@vue/reactivity';
const todoList = ref([]);

const fetchTodoList = () => {
  const savedTodoList = JSON.parse(localStorage.getItem('todoList'));
  if (savedTodoList) {
    todoList.value = savedTodoList;
  }
};

fetchTodoList();

watch(todoList,() => {setTodoListStorage();},{deep : true,});

const todoCompleted = computed(() => {
  return todoList.value.every((todo) => todo.isComplete);
});

const setTodoListStorage = () => {
  localStorage.setItem('todoList', JSON.stringify(todoList.value));
};

const todoCreate = (todo) => {
  todoList.value.push({
    'id':uid(),
    'todo':todo,
    'isComplete':null,
    'isEditing':null,
  });
};

const toggleTodoComplete = (todoPos) => {
  todoList.value[todoPos].isComplete = !todoList.value[todoPos].isComplete;
};

const toggleTodoEdit = (todoPos) => {
  todoList.value[todoPos].isEditing = !todoList.value[todoPos].isEditing;
};

const todoUpdate = (todoVal, todoPos) => {
  todoList.value[todoPos].todo = todoVal;
};

const todoDelete = (todoID) => {
  todoList.value = todoList.value.filter((todo) => (todo.id != todoID));
};
</script>

<template>
  <main>
    <h1>Create ToDo</h1>
    <TodoCreator @todo-create="todoCreate"/>
    <ul class="todo-list" v-if="todoList.length > 0">
      <TodoItem v-for="(todo,index) in todoList" :todo="todo" :index="index" @toggle-complete="toggleTodoComplete" @toggle-edit="toggleTodoEdit" @update-todo="todoUpdate" @todo-delete="todoDelete"></TodoItem>
    </ul>

    <p class="todos-msg" v-else>
      <Icon icon="noto-v1:sad-but-relieved-face" />
      <span>You have no todo's to complete! Add one!</span>
    </p>
    <p v-if="todoCompleted && todoList.length > 0" class="todos-msg">
      <Icon icon="noto-v1:party-popper" />
      <span>You have completed all your todos!</span>
    </p>
  </main>
</template>

<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }

  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>
