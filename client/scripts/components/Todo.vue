<template>
  <transition name='fade'>
    <article
      class="todo__card"
    >
      <p
        @click="startEditing"
        v-show="!editing"
        class="todo__description"
        :class="{ 'todo__description--complete': todo.done}"
      >
        {{todo.description}}
      </p>

      <input
        v-model="todo.description"
        v-show="editing"
        @blur="doneEditing"
        @keyup.enter="doneEditing"
        type="text"
        class="todo__description--editing"
        ref="editingDescription"
      />

      <label
        class="todo__complete--label"
      >
        <span class="todo__complete--complete" v-if="todo.done">Completed</span>
        <span class="todo__complete--incomplete" v-if="!todo.done">Incomplete</span>

        <input
          :checked="todo.done"
          type="checkbox"
          @click="toggleTodoComplete"
          class="todo__complete"
          :class="{ complete: todo.done }"
        />
      </label>

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

    startEditing() {
      this.editing = true;
      this.nextTick(() => {
        this.$refs.editingDescription.focus();
      });
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
  margin: 2rem 1rem;
  padding: 2.5rem;
  position: relative;

  &:hover {
    box-shadow: 0 14px 28px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
  }

  .todo__description {
    display: inline-block;
    margin-bottom: 1rem;
    line-height: 1.5rem;

    &--editing {
      display: inline-block;
      font-size: 1rem;
      margin-bottom: 1rem;
      padding: 0.65rem;
      width: 100%;
    }

    &--complete {
      color: $color-gray;
      text-decoration: line-through;
    }
  }

  .todo__complete--label {
    margin: 1rem;
    padding: 1rem;
    position: absolute;
    right: 0.5rem;
    bottom: 0.5rem;

    @media screen and(max-width: 768px) {
      right: 0;
    }

    .todo__complete {
      display: none;
    }

    .todo__complete--complete {
      color: $color-gray;
    }

    .todo__complete--complete::after,
    .todo__complete--incomplete::after {
      content: '';
      display: inline-block;
      height: 1.125rem;
      width: 1.125rem;
      border: 1px solid $color-gray;
      border-radius: 3px;
      vertical-align: middle;
      margin-left: 1rem;
    }

    .todo__complete--complete::after {
      content: '\f00c';
      color: green;
      font-family: 'FontAwesome';
      padding-left: 1px;
    }
  }

  .btn {
    display: block;
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
