<template>
  <div class="column is-4 is-offset-4">
    <b-field>
      <b-input placeholder="What needs to be done?"
        type="text"
        icon="check"
        size="is-large"
        v-model="newTodo"
        @keyup.native.enter="addTodoAndClearNewTodo">
      </b-input>
    </b-field>

    <div v-for="(todo, index) in todos" :key="index">
      <b-field class="is-pulled-left handle">
        <label class="subtitle is-3">
          {{todo.text}}
          <input type="text" :value="todo.text" @input="updateTodo(todo, $event.target.value)">
        </label>
      </b-field>
      <a class="delete is-pulled-right is-medium" @click="removeTodo(todo)"></a>
      <div class="is-clearfix"></div>
    </div>
  </div>
</template>

<script>
import Firebase from 'firebase'
require('dotenv').config()
var config = {
  apiKey: `process.env.API_KEY`,
  authDomain: `process.env.AUTH_DOMAIN`,
  databaseURL: 'https://premidterm.firebaseio.com',
  projectId: `process.env.PROJECT_ID`,
  storageBucket: `process.env.STORAGE_BUCKET`,
  messagingSenderId: `process.env.MESSAGING_SENDER_ID`
}
let app = Firebase.initializeApp(config)
let db = app.database()
let todosRef = db.ref('todos')
export default {
  name: 'HelloWorld',
  firebase: {
    todos: todosRef
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      newTodo: ''
    }
  },
  methods: {
    addTodoAndClearNewTodo () {
      todosRef.push({
        text: this.newTodo,
        done: false
      })
      this.newTodo = ''
    },
    removeTodo (todo) {
      todosRef.child(todo['.key']).remove()
    },
    updateTodo (todo, text) {
      todosRef.child(todo['.key'] + '/text').set(text)
    }
  }
}
</script>

<style scoped>
h1, h2 {
  font-weight: normal;
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
