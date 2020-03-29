<template>
  <div id="app">
    <Header />
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Todos from "./components/Todos";
import Header from "./components/layout/header";
import AddTodo from "./components/AddTodo";
import axios from "axios";
export default {
  name: "App",
  components: {
    Todos,
    Header,
    AddTodo
  },
  methods: {
    deleteTodo(id) {
      axios
        .delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        .then(() => {
          this.todos = this.todos.filter(todo => todo.id !== id);
        });
    },
    addTodo(newTodo) {
      const { title, completed } = newTodo;
      axios
        .post("https://jsonplaceholder.typicode.com/todos", {
          title,
          completed
        })
        .then(res => {
          this.todos = [...this.todos, res.data];
        })
        .catch(err => console.log(err));
    }
  },
  created() {
    axios
      .get("https://jsonplaceholder.typicode.com/todos?_limit=10")
      .then(res => {
        this.todos = res.data;
      });
  },
  data() {
    return {
      todos: []
    };
  }
};
</script>

<style>
.btn {
  display: inline-block;
  border: none;
  background: gray;
  color: white;
  padding: 7px 20px;
  cursor: pointer;
}
</style>
