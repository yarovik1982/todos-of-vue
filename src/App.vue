<template>
  <app-header />

  <app-filters :activeFilter="activeFilter" @setFilter="setFilter" />

  <main class="app-main">
    <app-todo-list :todos="filteredTodos" @removeTodo="removeTodo" @toggleTodo="toggleTodo"/>

    <app-add-todo @addTodo="addTodo" />
  </main>

  <app-footer :stats="stats" />
</template>
<script lang="ts">
import { defineComponent } from "vue";
import AppHeader from "@/components/AppHeader.vue";
import AppFilters from "./components/AppFilters.vue";
import AppTodoList from "./components/AppTodoList.vue";
import AppAddTodo from "./components/AppAddTodo.vue";
import AppFooter, { Stats } from "./components/AppFooter.vue";
import { Todo } from "./types/Todo";
import { Filter } from "./types/Filter";

interface State {
  todos: Todo[],
  activeFilter:Filter,
}
export default defineComponent({
  components: { AppHeader, AppFilters, AppTodoList, AppAddTodo, AppFooter },
  data(): State {
    return {
      todos: [
        { id: 0, text: "Learn the basics of Vue", completed: true },
        { id: 1, text: "Learn the basics of Typescript", completed: false },
        { id: 2, text: "Subscribe to the channel", completed: false },
      ],
      activeFilter:'All',
    };
  },
  computed:{
    filteredTodos(): Todo[]{
      switch(this.activeFilter){
        case 'Active':
          return this.activeTodos
        case 'Done':
          return this.doneTodos
        case 'All':
        default:
          return this.todos
      }
    },
    stats():Stats{
      return{
        active:this.activeTodos.length,
        done:this.doneTodos.length,
      }
    },
    activeTodos():Todo[]{
      return this.todos.filter(todo => !todo.completed)
    },
    doneTodos():Todo[]{
      return this.todos.filter(todo => todo.completed)
    }
  },
  methods: {
    toggleTodo(id:number){
      const targetTodo = this.todos.find((todo:Todo) => todo.id === id)
      if(targetTodo){
         targetTodo.completed = !targetTodo.completed
      }      
   },
   removeTodo(id:number){
      this.todos = this.todos.filter((todo:Todo) => todo.id !== id)
   },
    addTodo(todo: Todo) {
      this.todos.push(todo)
    },
    setFilter(filter: Filter){
      this.activeFilter = filter
    },
  },
});
</script>
