<template>
  <main>
    <TodoInput
      @createTodo="addTodo"
    />

    <section class="todo-list">
      <Todo
        v-for="todo in todos"
        :key="todo.id"
        :todo="todo"
        @todoToggled="markTodoComplete(todo)"
        @remove="deleteTodo(todo)"
        @todoEdited="doneEditingTodo(todo)"
      />
    </section>

  </main>
</template>

<script>
import TodoInput from './TodoInput.vue';
import Todo from './Todo.vue';

export default {
  name: 'app',

  components: {
    TodoInput,
    Todo,
  },

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

    addTodo(todo) {
      todo.id = this.todos.length;

      this.todos.push(todo);
      this.storeTodoOnServer(todo);
      this.description = '';
    },

    deleteTodo(todo) {
      const id = parseInt(todo.id);

      this.todos = this.todos.filter(todo => todo.id !== id);

      this.deleteTodoFromServer(id);
    },

    _updateTodo(todo) {
      const i = this.todos.indexOf(todo.id);

      if (i !== -1) {
        this.todos[i] = todo;
      }
      return null;
    },

    markTodoComplete(todo) {
      todo.done = !todo.done;

      this._updateTodo(todo);
      this.updateTodoOnServer(todo);
    },

    doneEditingTodo(todo) {
      this._updateTodo(todo);
      this.updateTodoOnServer(todo);
    },

    storeTodoOnServer(todo) {
      fetch('http://localhost:8004/api/todos', {
        method: 'post',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(todo),
      }).catch(err => console.error('an error occurred ', err));
    },

    deleteTodoFromServer(todoId) {
      fetch(`http://localhost:8004/api/todos/${todoId}`, {
        method: 'delete',
      }).catch(err => console.error('an error occurred ', err));
    },

    updateTodoOnServer(todo) {
      fetch(`http://localhost:8004/api/todos/${todo.id}`, {
        method: 'put',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(todo),
      }).catch(err => console.error('an error occurred ', err));
    },
  },
};
</script>

<style lang='scss'>
@import '../../styles/reset.css';
@import '../../styles/_config.scss';

body {
  font-family: $primary-font;
  font-size: 16px;
}

.btn {
  background-color: $color-dark-blue;
  color: white;
  font-size: 1.125rem;
  font-weight: 500;
  margin-top: 1rem;
  padding: 0.5rem 1rem;
  transition: all 0.125s ease;

  &:hover,
  &:focus {
    background-color: lighten($color-dark-blue, 10%);
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.19), 0 6px 6px rgba(0, 0, 0, 0.23);
  }

  &:active,
  &:focus:active {
    box-shadow: none;
    background-color: darken($color-dark-blue, 4%);
    color: #f38b00;
  }

  &:disabled {
    background: $color-white;
    color: #686c6f;
    border: 1px solid #686c6f;
    pointer-events: none;
  }
}
</style>
