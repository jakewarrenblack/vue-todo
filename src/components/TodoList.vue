<template>
  <div>
    <b-card 
      header="TodoList" 
      header-tag="header"
    >
    <b-list-group>
      <TodoItem 
        v-for="item in list" 
        :key="item.id" 
        :todo="item"
        v-on:todo-completed="completeTodo"
      />
    </b-list-group>

      <template v-slot:footer>
        <input type="text" v-model="todoText" v-on:keyup.enter="addNewTodo()" />
        <b-button class="float-end" variant="primary" @click="addNewTodo()">Add</b-button>
      </template>
    </b-card>
  </div>
</template>

<script>
import TodoItem from '@/components/TodoItem.vue'

export default {
  name: 'TodoList',
  components: {
    TodoItem
  },
  data() {
    return {
      list: [
        {
          id: 1,
          text: "Clean the house",
          done: true
        },
        {
          id: 2,
          text: "Buy milk!",
          done: false
        },
        {
          id: 3,
          text: "Create todo app using Vue",
          done: false
        }
      ],
      todoText: ""
    }
  },
  methods: {
    addNewTodo() {
      if(!this.todoText){
        alert("Please enter some text");
        return;
      }

      const newId = Math.max.apply(null, this.list.map(t => t.id)) + 1;

      this.list.push({
        id: newId,
        text: this.todoText,
        done: false
      });

      this.todoText = "";

    },
    completeTodo(todo){
      const todoIndex = this.list.indexOf(todo);
      this.list[todoIndex].done = true;
    }
  }
}
</script>

<style>
</style>
