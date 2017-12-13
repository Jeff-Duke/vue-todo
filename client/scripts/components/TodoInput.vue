<template>
  <section>
    <label> Todo
      <input
        v-model="description"
        placeholder="Enter a Todo"
        @keyup.enter="createTodo"
        type="text"
      />
    </label>
    <button
      @click="createTodo"
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
