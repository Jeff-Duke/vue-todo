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
      </article>
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

    addTodo(todo) {
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
      this.editingTodo = {};

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