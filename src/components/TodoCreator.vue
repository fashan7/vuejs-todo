<script setup>
import { reactive } from 'vue';
import TodoButton from './TodoButton.vue';

const emit = defineEmits(["todo-create"])

const todoState = reactive({
    todo: "",
    isInvalid: null,
    errorMsg:"",
});

const createTodo = () => {
    todoState.isInvalid = false;
    if (todoState.todo != "") {
        emit("todo-create", todoState.todo);
        todoState.todo = "";
        return;
    }
    todoState.isInvalid = true;
    todoState.errorMsg = "Textbox left empty!";
};
</script>

<template>
    <div class="input-wrap" :class="{ 'input-err':todoState.isInvalid }">
        <input type="text" v-model="todoState.todo"/>
        <TodoButton @click="createTodo()">Create ToDo</TodoButton>
    </div>
    <span v-show="todoState.isInvalid" class="err-msg">{{ todoState.errorMsg }}</span>
</template>



<style lang="scss" scoped>
.input-wrap {
    display: flex;
    transition: 250ms ease;
    border: 2px solid #41b080;

    &.input-err {
        border-color: red;
    }

    &:focus-within {
        box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
        0 -2px 4px -2px rgb(0 0 0 / 0.1);
    }

    input {
        width: 100%;
        padding: 8px 6px;
        border: none;
        &:focus {
            outline: none;
        }
    }
}

.err-msg {
    margin-top: 6px;
    font-size: 12px;
    text-align: center;
    color: red;
}
</style>