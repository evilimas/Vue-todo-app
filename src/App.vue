<script setup>
import { ref, onMounted, computed, watch } from 'vue';

const todos = ref([]);
const name = ref('');

const input_content = ref('');
const input_category = ref(null);

const todo_asc = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);
watch(name, (newVal) => {
  localStorage.setItem('name', newVal);
});
onMounted(() => {
  name.value = localStorage.getItem('name') || '';
});
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
          <label for="">
            <input
              type="radio"
              name="category"
              value="business"
              v-model="input_category"
            />
            <span class="bubble business"></span>
            <div>Business</div>
          </label>
          <label for="">
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
      </form>
    </section>
  </main>
</template>
