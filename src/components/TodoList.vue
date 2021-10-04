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
        v-on:todo-deleted="deleteTodo"
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
      // find the object at this index, and set its 'done' attribute to true
      const todoIndex = this.list.indexOf(todo);
      this.list[todoIndex].done = true;
      this.saveList();
    },
    deleteTodo(todo){
      // find the object at this index, and splice it from the array
      const todoIndex = this.list.indexOf(todo);
      console.log(todoIndex)
      this.list.splice(todoIndex,1);
      this.saveList();
    },
    saveList(){
      localStorage.setItem('todo_list', JSON.stringify(this.list));
    }
  },
  watch:{
    // add a watch on our 'list' defined above
    list: {
      // so we tell vue to 'watch' our list, and if something related to the list changes, run our handler() method
      handler() {
        console.log('something changed!');
        // localStorage can only save strings, have to use JSON.stringify on it
        this.saveList();
      }
    }
  },
  // mounted is called when the app has been created within the DOM
  mounted(){
    console.log('app mounted');
    if(localStorage.getItem('todo_list')){
      // the array was stringified, so use JSON.parse to convert it back into JSON
      this.list = JSON.parse(localStorage.getItem('todo_list'));
    }
  }
}
</script>

<style>
</style>
