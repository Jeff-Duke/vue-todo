<template>
  <main>
    <TodoInput
      @createTodo="addTodo"
      :error="error"
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

const ERROR_ADDING = 'There was an error adding the to-do to the server';
const ERROR_UPDATING = 'There was an error while updating the to-do';
const ERROR_FETCHING = 'There was an error retrieving your to-dos';
const ERROR_DELETING = 'There was an error while deleting the to-do';

export default {
  name: 'app',

  components: {
    TodoInput,
    Todo,
  },

  data() {
    return {
      todos: [],
      error: '',
    };
  },

  mounted: function loadTodosOnMount() {
    this.fetchTodos();
  },

  methods: {
    addTodo(todo) {
      todo.id = this.todos.length;

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

    fetchTodos() {
      fetch('http://localhost:8004/api/todos')
        .then(response => response.json())
        .then(data => (this.todos = data))
        .catch(err => (this.error = ERROR_FETCHING));
    },

    storeTodoOnServer(todo) {
      fetch('http://localhost:8004/api/todos', {
        method: 'post',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(todo),
      })
        .catch(err => {
          this.error = ERROR_ADDING;
          console.error('an error occurred ', err);
        })
        .then(() => this.fetchTodos());
    },

    deleteTodoFromServer(todoId) {
      fetch(`http://localhost:8004/api/todos/${todoId}`, {
        method: 'delete',
      }).catch(err => {
        this.error = ERROR_DELETING;
        console.error('an error occurred ', err);
      });
    },

    updateTodoOnServer(todo) {
      fetch(`http://localhost:8004/api/todos/${todo.id}`, {
        method: 'put',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(todo),
      }).catch(err => {
        this.error = ERROR_UPDATING;
        console.error('an error occurred ', err);
      });
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
  color: $color-white;
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
    color: $color-orange;
  }

  &:disabled {
    background: $color-white;
    color: $color-dark-gray;
    border: 1px solid $color-dark-gray;
    pointer-events: none;
  }
}

.todo-list {
  margin: 2rem auto;
  max-width: 80vw;

  @media screen and(max-width: 480px) {
    max-width: 100vw;
  }
}
</style>
