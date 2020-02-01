/* eslint-disable no-console */
/* eslint-disable no-console */
<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Todos from "../components/Todos";
import AddTodo from "../components/AddTodo";
import axios from "axios";

export default {
  name: "Home",
  components: {
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: []
    };
  },
  methods: {
    deleteTodo(id) {
      //sweet alert for pop delete
      if (window.navigator.onLine) {
        axios
          .delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
          .then(res => (this.todos = this.todos.filter(todo => todo.id !== id)))
          .catch(err => console.log(err));
      } else {
        //code to delete not from localStorage here
        localStorage.removeItem(id);
        console.log("you deleted something");
      }
    },
    addTodo(newTodo) {
      if (window.navigator.onLine) {
        const { title, completed } = newTodo;
        axios
          .post("https://jsonplaceholder.typicode.com/todos", {
            title,
            completed
          })
          .then(res => (this.todos = [...this.todos, res.data]))
          .catch(err => console.log(err));
      } else {
        //am not sure on this code
        const { id } = newTodo;
        window.localStorage.setItem(id, JSON.stringify(newTodo));
      }
    }
  },
  created() {
    if (window.navigator.onLine) {
      axios
        .get("https://jsonplaceholder.typicode.com/todos?_limit=5")
        .then(res => (this.todos = res.data))
        .catch(err => console.log(err));
    } else {
      alert("you are now offline");
      //localStorage code here
      //retrive for view
    }
  }
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}

.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}

.btn:hover {
  background: #666;
}
</style>
