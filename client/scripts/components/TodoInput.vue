<template>
  <section class="todo__header">
    <label> Add a To-do:
      <input
        v-model="description"
        placeholder="To-do Description"
        @keyup.enter="createTodo"
        type="text"
        class="todo__input"
        ref="todo__input"
      />
    </label>
    <button
      @click="createTodo"
      class="btn btn__primary"
      :disabled="!description"
    >
      Add To-do
    </button>
    <h2 v-show="errorMessage" class="todo__error">{{errorMessage}}</h2>
  </section>
</template>
<script>
export default {
  name: 'TodoInput',
  props: ['error'],
  data() {
    return {
      description: '',
      todo: {},
      errorMessage: this.error,
    };
  },
  mounted() {
    this.$refs.todo__input.focus();
  },
  methods: {
    createTodo() {
      if (this.description === '') {
        this.errorMessage = 'You must enter a description';
        return null;
      }
      this.errorMessage = '';

      const todo = {
        description: this.description,
        done: false,
        created: Date.now(),
      };
      this.description = '';
      this.$refs.todo__input.focus();

      this.$emit('createTodo', todo);
    },
  },
};
</script>
<style lang="scss">
@import '../../styles/_config.scss';

.todo__header {
  align-items: center;
  background-color: $color-light-blue;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
  display: flex;
  flex-direction: column;
  justify-content: center;
  min-height: 8rem;
  padding: 2rem;

  label {
    font-size: 1.125rem;
    padding-left: 0.5rem;
  }

  .todo__input {
    border: 2px solid $color-dark-blue;
    border-radius: 2px;
    display: block;
    font-size: 1.125rem;
    font-family: sans-serif;
    margin: 0.5rem 0;
    padding: 0.5rem;
    width: 20rem;

    &::placeholder {
      font-size: 0.8rem;
    }
  }

  .todo__error {
    color: $color-red;
    font-size: 1.25rem;
    margin-top: 1rem;
  }
}
</style>
