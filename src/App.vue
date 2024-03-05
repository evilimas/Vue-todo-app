<script setup>
import { ref, onMounted, computed, watch } from 'vue';

const todos = ref([]);
const name = ref('');

const input_content = ref('');
const input_category = ref(null);

const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);
const addTodo = () => {
  if (input_content.value.trim() === '' || input_category.value === null) {
    return;
  }
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime(),
  });
  input_content.value = '';
  input_category.value = null;
};
const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        Whats up, <input type="text" placeholder="Name here" v-model="name" />
      </h2>
    </section>
    <section class="create-todo">
      <h3>CREATE TO DO</h3>
      <form @submit.prevent="addTodo">
        <h4>Whats on your todo list?</h4>
        <input
          type="text"
          placeholder="e.g. clean your room"
          v-model="input_content"
        />
        <h4>Pick a category</h4>
        <div class="options">
          <label>
            <input
              type="radio"
              name="category"
              value="business"
              v-model="input_category"
            />
            <span class="bubble business"></span>
            <div>Work</div>
          </label>
          <label>
            <input
              type="radio"
              name="category"
              value="personal"
              v-model="input_category"
            />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>
        <input type="submit" value="Add todo" />
      </form>
    </section>
    <section class="todo-list">
      <h3>YOUR TODO LIST</h3>
      <div class="list">
        <div
          v-for="todo in todos_asc"
          :key="todo.createdAt"
          :class="`todo-item ${todo.done && 'done'}`"
        >
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${todo.category}`"></span>
          </label>
          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>
          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Erase</button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'montserrat', sans-serif;
}

input:not([type='radio']):not([type='checkbox']),
button {
  appearance: none;
  border: none;
  outline: none;
  background: none;
  cursor: initial;
}

body {
  background: #e2e2e2;
  color: #313154;
}
main {
  max-width: 800px;
  margin: auto;
}

section {
  margin-top: 2rem;
  margin-bottom: 2rem;
  padding-left: 1.5rem;
  padding-right: 1.5em;
}

h3 {
  color: #313154;
  font-size: 1rem;
  font-weight: 400;
  margin-bottom: 0.5rem;
}

h4 {
  color: #707070;
  font-size: 0.875rem;
  font-weight: 700;
  margin-bottom: 0.5rem;
}

.greeting .title {
  display: flex;
}

.greeting .title input {
  margin-left: 0.5rem;
  flex: 1 1 0%;
  min-width: 0;
}

.greeting .title,
.greeting .title input {
  color: #313154;
  font-size: 1.5rem;
  font-weight: 700;
}

.create-todo input[type='text'] {
  display: block;
  width: 100%;
  font-size: 1.125rem;
  padding: 1rem 1.5rem;
  color: #313154;
  background-color: #fff;
  border-radius: 0.5rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  margin-bottom: 1.5rem;
}

.create-todo .options {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 1rem;
  margin-bottom: 1.5rem;
}

.create-todo .options label {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 1.5rem;
  background-color: #fff;
  border-radius: 0.5rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  cursor: pointer;
}

input[type='radio'],
input[type='checkbox'] {
  display: none;
}

.bubble {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  border: 2px solid #b60000;
  box-shadow: 0px 0px 4px rgba(248, 96, 96, 0.75);
}

.bubble.personal {
  border-color: #008643;
  box-shadow: 0px 0px 4px rgba(4, 172, 69, 0.75);
}

.bubble::after {
  content: '';
  display: block;
  opacity: 0;
  width: 0px;
  height: 0px;
  background-color: #b60000;
  box-shadow: 0px 0px 4px rgba(248, 96, 96, 0.75);
  border-radius: 50%;
  transition: 0.2s ease-in-out;
}

.bubble.personal::after {
  background-color: #008643;
  box-shadow: 0px 0px 4px rgba(4, 172, 69, 0.75);
}

input:checked ~ .bubble::after {
  width: 10px;
  height: 10px;
  opacity: 1;
}

.create-todo .options label div {
  color: #313154;
  font-size: 1.125rem;
  margin-top: 1rem;
}

.create-todo input[type='submit'] {
  display: block;
  width: 100%;
  font-size: 1.125rem;
  padding: 1rem 1.5rem;
  color: #fff;
  background-color: #008643;
  border-radius: 0.5rem;
  box-shadow: 0px 0px 4px rgba(4, 172, 69, 0.75);
  cursor: pointer;
  transition: 0.2s ease-in-out;
}

.create-todo input[type='submit']:hover {
  opacity: 0.75;
}

.todo-list .list {
  margin: 1rem 0;
}

.todo-list .todo-item {
  display: flex;
  align-items: center;
  background-color: #fff;
  padding: 1rem;
  border-radius: 0.5rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  margin-bottom: 1rem;
}

.todo-item label {
  display: block;
  margin-right: 1rem;
  cursor: pointer;
}

.todo-item .todo-content {
  flex: 1 1 0%;
}

.todo-item .todo-content input {
  color: #313154;
  font-size: 1.125rem;
}

.todo-item .actions {
  display: flex;
  align-items: center;
}

.todo-item .actions button {
  display: block;
  padding: 0.5rem;
  border-radius: 0.25rem;
  color: #fff;
  cursor: pointer;
  transition: 0.2s ease-in-out;
}

.todo-item .actions button:hover {
  opacity: 0.75;
}

.todo-item .actions .edit {
  margin-right: 0.5rem;
  background-color: #008643;
}

.todo-item .actions .delete {
  background-color: #bb0502;
}

.todo-item.done .todo-content input {
  text-decoration: line-through;
  color: #707070;
}
</style>
