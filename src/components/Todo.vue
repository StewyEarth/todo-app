<template>
  <div id="todo">
    <h1>Add things To do 🔥</h1>
    <input type="“text”" class="“nes-input”" placeholder="Add todo…" v-model="newTodo" v-on:keyup.enter="addTodo" />
      <div v-for="(todo,index) in todos" v-bind:key="todo.id" class="todo-item">
        
        <div class="todo-title" :class="{ 'todo-completed': todo.completed }" v-on:click="ChangeTodoCompletion(index)">{{ todo.title }}</div>
        <div class="todo-remove" v-on:click="removeTodo(index)">&times;</div>

      </div>
  </div>
</template>

<script>
export default {
  name: "todo-list",
  data () {
    return {
      newTodo: "",
      todos: [
        {
          id: 0,
          title: "Yeet",
          completed: true,
        },
        {
          id: 1,
          title: "Skeet",
          completed: false,
        },
        {
          id: 2,
          title: "Delete",
          completed: false,
        }
      ]
    };
  },
  methods: {
    addTodo (event) {
      // `this` inside methods points to the Vue instance
      if(this.newTodo != ""){
        this.todos.push({
          title: this.newTodo,
        id: new Date().getTime()
      });
      this.newTodo = ""
      }
    },
    removeTodo (index) {
      // `this` inside methods points to the Vue instance
      this.todos.splice(index,1)
    },
    ChangeTodoCompletion (index) {
      let status = false
      if(!this.todos[index].completed){
        status = true
      }
      this.todos[index].completed = status;
    }
  }
};
</script>

<style scoped>
#todo{
  width: 60%;
  margin: 0 auto;
}
input {
  padding: 1em;
  width: 100%;
  box-sizing: border-box;
  margin-bottom: 3em;
}
.todo-item{
  display: flex;
  justify-content: space-between;
}
.todo-title{
  cursor: pointer;
}
.todo-remove{
  color: red;
  cursor: pointer;
  font-size: 1.5rem;
  transition: color .3s;
}
.todo-remove:hover{
  color: black;
}
.todo-completed{
  text-decoration: line-through;
  color: gray;
  text-decoration-color: black;
}
</style>