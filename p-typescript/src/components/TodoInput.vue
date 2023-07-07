<template>
  <div>
    <label for="todo-input">Today's Todo : </label>
    <input type="text" 
      id="todo-input"   
      :value="item"
      @input="handleInput" />
    <button type="button" @click="addTodo">Add</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  setup() {
    return {}
  },
  props: {
    item: {
      type: String,
      required: true,
    },    
  },
  methods: {
    handleInput(event: InputEvent) {
      // event.target.value =>  타입 추론  type  추론의문제가 발생
      // this.$emit('inputValue', event.target.value);      
      const eventTarget = event.target as HTMLInputElement;
      this.$emit("inputTodo", eventTarget.value);
    },
    addTodo() {            
      this.$emit("addTodo")
    }
  }

})
</script>


<style scoped></style>