<template>
  <section class="todo__header">
    <label> Add a Todo:
      <input
        v-model="description"
        placeholder="Todo Description"
        @keyup.enter="createTodo"
        type="text"
        class="todo__input"
      />
    </label>
    <button
      @click="createTodo"
      class="btn"
    >
      Add Todo
    </button>
  </section>
</template>
<script>
export default {
  name: 'TodoInput',
  data() {
    return {
      description: '',
      todo: {},
    };
  },
  methods: {
    createTodo() {
      const todo = {
        description: this.description,
        done: false,
        id: Date.now(),
      };
      this.description = '';
      this.$emit('createTodo', todo);
    },
  },
};
</script>
<style lang="scss">
.todo__header {
  min-height: 8rem;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 2rem;
  flex-direction: column;

  label {
    padding-left: 0.5rem;
  }

  .todo__input {
    display: block;
    font-size: 1.125rem;
    font-family: sans-serif;
    margin: 0.5rem 0;
    padding: 0.5rem;
  }

  .btn {
    background-color: #003b49;
    color: white;
    font-size: 1.125rem;
    font-weight: 500;
    margin-top: 1rem;
    padding: 0.5rem 1rem;
    transition: all 0.125s ease;

    &:hover,
    &:focus {
      background-color: lighten(#003b49, 30%);
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.19), 0 6px 6px rgba(0, 0, 0, 0.23);
    }

    &:active,
    &:focus:active {
      box-shadow: 0;
      background-color: darken(#003b49, 10%);
      color: #f38b00;
    }
  }
}
</style>
