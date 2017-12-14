<template>
  <transition>
    <article class="todo__card">
      <h1
        @click="editing = true"
        v-show="!editing"
        class="todo__description"
        :class="{ 'todo__description--complete': todo.done}"
      >
        {{todo.description}}
      </h1>
      <input
        v-model="todo.description"
        v-show="editing"
        @blur="doneEditing"
        @keyup.enter="doneEditing"
        type="text"
        class="todo__description--editing"
      />
      <input
        :checked="todo.done"
        type="checkbox"
        @click="toggleTodoComplete"
        class="todo__complete"
        :class="{ complete: todo.done}"
      />
      <button
        @click="removeTodo"
        class="btn"
      >
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
      complete: null,
    };
  },
  methods: {
    toggleTodoComplete() {
      this.$emit('todoToggled', this);
    },
    removeTodo() {
      this.$emit('remove', this);
    },
    doneEditing() {
      this.editing = false;
      this.$emit('todoEdited', this);
    },
  },
};
</script>
<style lang="scss">
@import '../../styles/_config.scss';
.todo__card {
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
  transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
  margin: 1rem;
  padding: 2rem;

  &:hover {
    box-shadow: 0 14px 28px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
  }

  .todo__description {
    display: inline-block;
    margin: 0.125rem;
    max-width: 85%;
    line-height: 1.25rem;

    &--editing {
      display: inline-block;
      font-size: 1rem;
      padding: 0.125rem;
      width: 85%;
    }

    &--complete {
      color: $color-gray;
      text-decoration: line-through;
    }
  }

  .todo__complete {
    float: right;
  }

  .btn {
    display: block;
  }
}
</style>
