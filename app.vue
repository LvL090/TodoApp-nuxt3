<script setup lang="ts">
const { data: todos, refresh } = useAsyncData ("todos", () => {
  return $fetch("/api/todo");
});
const input = ref("");

const addTodo = async () => {
  if (!input) return;
  await $fetch("/api/todo", { method: "post", body: { item: input.value } });
  refresh();
};

const updateTodo = async (id) => {
  await $fetch(`/api/todo/${id}`, { method: "put" });
  refresh();
};

const deleteTodo = async (id) => {
  await $fetch(`/api/todo/${id}`, { method: "delete" });
  refresh();
};
</script>

<template>
  <div class="container">
    <NCard class="cards">
      <h1>My Todos</h1>
      <div class="add-todo">
        <input
          placeholder="Add a new todo..."
          v-model="input"
          @keyup.enter="addTodo"
        />
        <NButton @click="addTodo">Add</NButton>
      </div>
      <NCard
        @click="updateTodo(todo.id)"
        class="card"
        v-for="todo in todos"
        :key="todo.is"
      >
        <h4 :class="todo.completed ? 'complete' : null">
          {{ todo.item }}
        </h4>
        <p @click="deleteTodo(todo.id)">X</p>
      </NCard>
    </NCard>
  </div>
</template>

<style scoped>
.container {
  padding: 2rem;
  margin: 0 auto;
  max-width: 50%;
}

.cards {
  padding: 2 rem;
}

input {
  outline: none;
}

.add-todo {
  display: flex;
  justify-content: space-between;
}

.card {
  padding: 0.5rem;
  margin-top: 1rem;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
}

.complete {
  text-decoration: line-through;
}
</style>
