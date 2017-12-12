<template>
  <section>
    <article v-for="todo in todos" :key="todo.id">
      <h1>{{todo.description}}</h1>
      <input type="checkbox" :checked="todo.done">
    </article>
  </section>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      todos: [],
    };
  },
  mounted: function loadTodosOnMount() {
    this.fetchTodos();
  },
  methods: {
    fetchTodos() {
      fetch('http://localhost:8004/api/todos')
        .then(response => response.json())
        .then(data => (this.todos = data));
    },
  },
};
</script>

<style>
div {
  font-size: 2rem;
  font-weight: bold;
  font-family: sans-serif;
}
</style>