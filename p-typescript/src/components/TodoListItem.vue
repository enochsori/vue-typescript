<template>  
    <li>
      <span :class="completedTodoItem" @click="completeTodo">{{ todoItem?.title }}</span>
      <button @click="removeTodo">Delete</button>
    </li>  
</template>

<script lang="ts">
import type { Todo } from '@/App.vue';
import { defineComponent, type PropType } from 'vue'


export default defineComponent({
  setup() {
    return {}
  },
  props: {
    todoItem: {
      type: Object as PropType<Todo>,
    },
    id : {
      type : Number,
    }
  },  
  methods: {
    removeTodo() {
      this.$emit('remove', this.id);
    },
    completeTodo () {      
      this.$emit('complete', this.id);
    }
  },
  computed: {
    // dynamically applying a class name
    completedTodoItem() : string | null {
      console.log(this.todoItem?.completed ? "completed" : null);
      return this.todoItem?.completed ? "completed" : null;
    }
  },
})
</script>

<style scoped>
li {
  list-style: none;
  width: 400px;
  display: flex;
  justify-content: space-between;
}
span {
  cursor: pointer;  
  
}
.completed {
  text-decoration: line-through;
}
</style>