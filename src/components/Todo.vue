<template>
  <div id="todo">
    <h1>Add things To do 🔥</h1>
    <div class="tabmenu">
      <button class=" tabbutton" :class="{'tabbutton--active':filter == 'all'}" v-on:click="filter = 'all'">All</button>
      <button class="tabbutton" :class="{'tabbutton--active':filter == 'completed' }"  v-on:click="filter = 'completed'">Completed</button>
      <button class="tabbutton" :class="{'tabbutton--active':filter == 'notcompleted'}"  v-on:click="filter = 'notcompleted'">Not Completed</button>
    </div>
    <input
      type="“text”"
      class="“nes-input”"
      placeholder="Add todo…"
      v-model="newTodo"
      v-on:keyup.enter="addTodo"
    />
    <div v-for="(todo,index) in todosFiltered" :key="index" class="todo-item">
      <div
        class="todo-title"
        :class="{ 'todo-completed': todo.completed }"
        v-on:click="ChangeTodoCompletion(index)"
      >{{ todo.title }}</div>
      <div class="todo-remove" v-on:click="removeTodo(index)">&times;</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "todo-list",
  data() {
    return {
      newTodo: "",
      filter: "all",
      todos: [
        {
          id: 0,
          title: "Yeet",
          completed: true
        },
        {
          id: 1,
          title: "Skeet",
          completed: false
        },
        {
          id: 2,
          title: "Delete",
          completed: false
        }
      ]
    };
  },
  methods: {
    addTodo() {
      // `this` inside methods points to the Vue instance
      if (this.newTodo != "") {
        this.todos.push({
          title: this.newTodo,
          id: new Date().getTime(),
          completed: false
        });
        this.newTodo = "";
      }
    },
    removeTodo(index) {
      // `this` inside methods points to the Vue instance
      this.todos.splice(index, 1);
    },
    ChangeTodoCompletion(index) {
      let status = false;
      if (!this.todos[index].completed) {
        status = true;
      }
      this.todos[index].completed = status;
    }
  },
  computed:{
    todosFiltered(){
      if (this.filter == "all"){
        return this.todos
      }else if (this.filter == "completed"){
        return this.todos.filter(todo => todo.completed)
      }else if(this.filter == "notcompleted"){
        return this.todos.filter(todo => !todo.completed)
      }
      return this.todos
    }
  }
};
</script>

<style scoped>
a {
  text-decoration: none;
}
#todo {
  width: 60%;
  margin: 0 auto;
}
input {
  padding: 1em;
  width: 100%;
  box-sizing: border-box;
  margin-bottom: 3em;
}
.todo-item {
  display: flex;
  justify-content: space-between;
}
.todo-title {
  cursor: pointer;
}
.todo-remove {
  color: red;
  cursor: pointer;
  font-size: 1.5rem;
  transition: color 0.3s;
}
.todo-remove:hover {
  color: black;
}
.todo-completed {
  text-decoration: line-through;
  color: gray;
  text-decoration-color: black;
}
.tabmenu{
  margin-bottom: 1em;
}
.tabbutton {
  border: none;
  background-color: transparent;
  color: #4a4a4a;
  margin-right: 1em;
  border-bottom: 2px solid transparent;
  padding-bottom: .2em;
}
.tabbutton--active {
  border-bottom-color: #00d1b2;
  color: #00d1b2;
}
.tabbutton:last-of-type {
  margin-right: 0;
}
</style>