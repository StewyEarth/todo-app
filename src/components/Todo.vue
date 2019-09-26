<template>
  <div id="todo">
    <h1>Add things To do 🔥</h1>
    <div class="tabmenu">
      <button
        class="tabbutton"
        :class="{'tabbutton--active':filter == 'all'}"
        v-on:click="filter = 'all'"
      >All</button>
      <button
        class="tabbutton"
        :class="{'tabbutton--active':filter == 'completed' }"
        v-on:click="filter = 'completed'"
      >Completed</button>
      <button
        class="tabbutton"
        :class="{'tabbutton--active':filter == 'notcompleted'}"
        v-on:click="filter = 'notcompleted'"
      >Not Completed</button>
    </div>
    <input
      type="text"
      class="addtodo"
      placeholder="Add todo…"
      v-model="newTodo"
      v-on:keyup.enter="addTodo"
    />
    <div v-for="(todo,index) in todosFiltered" :key="index" class="todo-item">
      <div
        class="todo-title"
        :class="{ 'todo-completed': todo.completed }"
        v-on:click="ChangeTodoCompletion(todo.id)"
      >{{ todo.title }}</div>
      <div class="todo-remove" v-on:click="removeTodo(todo.id)">&times;</div>
    </div>
    <div class="todo-bottom">
      <div>
        <input v-on:change="changeall" type="checkbox" name id="completeall" :checked="anyRemaining" />
        <label for="completeall">Check/uncheck all</label>
      </div>
      <div>
        <p>items left: {{remaining}}</p>
      </div>
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
          title: "Thing 1",
          completed: true
        },
        {
          id: 1,
          title: "Thing 2",
          completed: false
        },
        {
          id: 2,
          title: "Thing 3",
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
    removeTodo(itemID) {
      this.todos.forEach((todo, index) => {
        if (todo.id == itemID) {
          this.todos.splice(index, 1);
        }
      });
      // this.todos.splice(index, 1);
    },
    ChangeTodoCompletion(itemID) {
      let status = false;
      this.todos.forEach((todo, index) => {
        if (todo.id == itemID) {
          if (!this.todos[index].completed) {
            status = true;
          }
          this.todos[index].completed = status;
        }
      });
    },
    changeall(){
      this.todos.forEach(todo=>{
        todo.completed = event.target.checked
      })
    }
  },
  computed: {
    todosFiltered() {
      if (this.filter == "all") {
        return this.todos;
      } else if (this.filter == "completed") {
        return this.todos.filter(todo => todo.completed);
      } else if (this.filter == "notcompleted") {
        return this.todos.filter(todo => !todo.completed);
      }
      return this.todos;
    },
    remaining() {
      return this.todos.filter(todo => !todo.completed).length;
    },
    anyRemaining() {
      return this.remaining == 0;
    }
  }
};
</script>

<style scoped>
a {
  text-decoration: none;
}
p {
  margin: 0;
}
#todo {
  width: 60%;
  margin: 0 auto;
}
.addtodo {
  padding: 1em;
  width: 100%;
  box-sizing: border-box;
  margin-bottom: 2em;
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
.todo-bottom {
  display: flex;
  justify-content: space-between;
  border-top: 1px solid black;
  margin-top: 1em;
  padding: 1em 0;
}
#completeall {
  margin: 0 0.5em 0 0;
}
.tabmenu {
  margin-bottom: 1em;
}
.tabbutton {
  cursor: pointer;
  border: none;
  background-color: transparent;
  color: #4a4a4a;
  margin-right: 1em;
  border-bottom: 2px solid transparent;
  padding-bottom: 0.2em;
  transition: all 0.3s;
}
.tabbutton:hover {
  border-bottom-color: #00d1b2;
  color: #00d1b2;
}
.tabbutton--active {
  border-bottom-color: #00d1b2;
  color: #00d1b2;
}
.tabbutton:last-of-type {
  margin-right: 0;
}
</style>