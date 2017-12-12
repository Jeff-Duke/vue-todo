<template>
  <main>
    <section>
      <label> Todo
        <input v-model="description" placeholder="Enter a Todo" type="text"/>
      </label>
      <button
        @click="addTodo"
      >
        Add Todo
      </button>
    </section>
    <section class="todo-list">
      <article v-for="todo in todos" class="todo-list__todo" :key="todo.id">
        <h1>{{todo.description}}</h1>
        <input
          @click="markTodoComplete(todo.id)" :checked="todo.done"
          type="checkbox"
        />
        <button @click="deleteTodo(todo.id)">
          Delete
        </button>
      </article>
    </section>
  </main>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      todos: [],
      description: '',
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

    addTodo() {
      const todo = {
        description: this.description,
        done: false,
        id: Date.now(),
      };

      this.todos.push(todo);
      this.storeTodoOnServer(todo);
      this.description = '';
    },

    deleteTodo(todoId) {
      const id = parseInt(todoId);

      this.todos = this.todos.filter(todo => todo.id !== id);

      this.deleteTodoFromServer(id);
    },

    _findTodo(todoId) {
      const id = parseInt(todoId);
      return this.todos.find(todo => todo.id === id);
    },

    markTodoComplete(todoId) {
      const todo = this._findTodo(todoId);
      todo.done = !todo.done;

      this.updateTodoOnServer(todo);
    },

    storeTodoOnServer(todo) {
      fetch('http://localhost:8004/api/todos', {
        method: 'post',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(todo),
      })
        .then(res => res.json())
        .then(res => console.log(res))
        .catch(err => console.error('an error occurred ', err));
    },

    deleteTodoFromServer(todoId) {
      fetch(`http://localhost:8004/api/todos/${todoId}`, {
        method: 'delete',
      })
        .then(res => res.json())
        .then(res => console.log(res))
        .catch(err => console.error('an error occurred ', err));
    },

    updateTodoOnServer(todo) {
      fetch(`http://localhost:8004/api/todos/${todo.id}`, {
        method: 'put',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(todo),
      })
        .then(res => res.json())
        .then(res => console.log(res))
        .catch(err => console.error('an error occurred ', err));
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