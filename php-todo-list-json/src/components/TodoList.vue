<template>
  <div class="hello">
    <h1>Hello World</h1>
    <ul>
      <li
        v-for="(todoElem, index) in todoList"
        :key="index"
      >
        {{ todoElem.text }}
      </li>
    </ul>
    <form @submit="createNewTask">
      <input type="text" name="text" v-model="newTodoText">
      <input type="submit" value="CREATE">
    </form>
  </div>
</template>

<script>

import axios from 'axios';

const API_URL = "http://localhost/";

export default {
  name: 'TodoList',
  data() {

    return {

      todoList: [],
      newTodoText: ""
    };
  },
  methods: {

    createNewTask(e) {

      e.preventDefault();

      const newTodoText = this.newTodoText;

      const params = { params: {
        "text": newTodoText
      }};

      axios.get(API_URL + "api_create_new_task.php", params)
           .then(() => {

            this.newTodoText = "";
            this.getAllData();
           });
    },
    getAllData() {
      
      axios.get(API_URL + "api.php")
           .then(res => {

            const data = res.data;

            this.todoList = data;
           });
    },
  },
  mounted() {
    
    this.getAllData();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
