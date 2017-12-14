<template>
  <transition>
    <article>
      <h1
        @click="editing = true"
        v-show="!editing"
      >
        {{todo.description}}
      </h1>
      <input
        v-model="todo.description"
        v-show="editing"
        @blur="doneEditing"
        @keyup.enter="doneEditing"
        type="text"
      />
      <input
        :checked="todo.done"
        type="checkbox"
        @click="toggleTodoComplete"
      />
      <button @click="removeTodo">
        Delete
      </button>
    </article>
  </transition>
</template>
<script>
export default {
  name: 'Todo',
  props: ['todo'],
  data() {
    return {
      editing: false,
    };
  },
  methods: {
    toggleTodoComplete() {
      this.$emit('todoToggled', this);
    },
    removeTodo() {
      this.$emit('remove', this);
    },
    editingTodo() {
      this.$emit('editingTodo', this);
    },
    doneEditing() {
      this.editing = false;
      this.$emit('todoEdited', this);
    },
  },
};
</script>
<style lang="scss">

</style>
