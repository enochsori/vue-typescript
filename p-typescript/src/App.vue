<template>
  <div>
    <header>
      <h1>Vue Todo with Typescript</h1>
    </header>

    <main>
      <TodoInput :item="todoText" @inputTodo="updateTodoText" @addTodo="addTodoItem"></TodoInput>
    </main>
    <div>
      <ul>
        <TodoListItem 
          v-for="todoItem in todoItems" 
          :key="todoItem.id" 
          :todoItem="todoItem"
          :id="todoItem.id"
          @remove="removeTodoItem"          
          @complete="completeTodo"
          >
      </TodoListItem>
      </ul>
    </div>

  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import TodoInput from './components/TodoInput.vue';
import TodoListItem from './components/TodoListItem.vue'

const STORAGE_KEY = 'vue-todo-ts-v1'

const storage = {
  save(todoItems : any[]) {        
    const parsed = JSON.stringify(todoItems);
    localStorage.setItem(STORAGE_KEY, parsed);
  },

  fetch() : Todo[] {
    const todoItems = localStorage.getItem(STORAGE_KEY) || "[]";
    const parsedTodoItems = JSON.parse(todoItems);    
    return parsedTodoItems ? parsedTodoItems : [];
  }
}

export interface Todo {
  title : string,
  completed : boolean,
  id: number,
}

export default defineComponent({
  setup() {
    return {};
  },
  components: {
    TodoInput, 
    TodoListItem,    
  },

  // vda
  data() {
    return {
      
      todoText: "",
      todoItems : [] as Todo[]
    }
  },
  methods: {
    updateTodoText(value: string) {      
      this.todoText = value;      
    },
    addTodoItem() {      
      if(this.todoText.length > 0) {                        
        this.todoItems.push({title: this.todoText, completed:false, id:this.todoItems.length});        
        storage.save(this.todoItems)
        this.initTodoText();
      }                
    },
    initTodoText() {
      this.todoText = '';
    },
    fetchTodoItems () {
      // sorting using sort api
      this.todoItems = storage.fetch().sort((a, b) => {
        if(a.title < b.title) {
          return -1
        }
        if(a.title > b.title) {
          return 1
        }
        return 0;
      })
    },
    removeTodoItem (id: number) {
      this.todoItems.forEach((todo, index) => {
        if(todo.id === id) {
        this.todoItems.splice(index, 1);
        }
      });      
      console.log(this.todoItems);
      // storage.save(remainTodoItems);      
      
      
    },
    completeTodo(id:number){
      this.todoItems.forEach(todo => {
        if(todo.id === id) {
          todo.completed = !todo.completed
        }
      })
      // console.log(this.todoItems[targetIndex].completed)
      // this.todoItems[targetIndex].completed = true;
    }
  }, 
  created () {    
      this.fetchTodoItems();    
  },
})
</script>

<style scoped></style>