<template>
  <div id="todo">
    <div class="todolists">
      <h4>Lists:</h4>
      <div v-for="(list,index) in lists" :key="index" class="todo-list">
        <button
          class="todo-list-btn tabbutton"
          :class="{'tabbutton--active':currentlist == index}"
          v-on:click="currentlist = index"
        >{{list.name}}</button>
        <button
          class="tabbutton listremove"
          v-on:click="removeList(list.id)"
          :class="{'hidden': lists.length == 1}"
        >&times;</button>
      </div>
      <input
        type="text"
        class="listaddinput"
        v-model="newList"
        v-on:blur="listAddMode = false"
        v-on:keyup.enter="addlist"
        placeholder="List name here"
        :class="{'hidden':listAddMode == false}"
      />
      <button class="tabbutton addlistbtn" v-on:click="listAddMode = true">+</button>
    </div>

    <h1 class="list-title">{{lists[this.currentlist].name}}</h1>
    <p class="instructions">Add things to do 🔥</p>
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
    <p v-if="todosFiltered.length == 0">No results found... add or complete something</p>
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
        <input
          v-on:change="changeall"
          type="checkbox"
          name
          id="completeall"
          :checked="noRemaining"
        />
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
      newList: "",
      newTodo: "",
      filter: "all",
      currentlist: 0,
      listAddMode: false,
      lists: [
        {
          id: 0,
          name: "Todo list",
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
        },
        // {
        //   id: 1,
        //   name: "Baskelisten",
        //   todos: [
        //     {
        //       id: 0,
        //       title: "Ralle 😭",
        //       completed: false
        //     },
        //     {
        //       id: 1,
        //       title: "Mikkel",
        //       completed: false
        //     },
        //     {
        //       id: 2,
        //       title: "Niklaz 😈",
        //       completed: true
        //     }
        //   ]
        // }
      ]
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo != "") {
        this.lists[this.currentlist].todos.push({
          title: this.newTodo,
          id: new Date().getTime(),
          completed: false
        });
        this.newTodo = "";
      }
    },
    addlist() {
      // `this` inside methods points to the Vue instance
      if (this.newList != "") {
        this.lists.push({
          id: new Date().getTime(),
          name: this.newList,
          todos: []
        });
        (this.listAddMode = false), (this.currentlist = this.lists.length - 1);
        this.newList = "";
      }
    },
    removeList(listID) {
      this.lists.forEach((list, index) => {
        if (list.id == listID) {
          this.lists.splice(index, 1);
        }
      });
      if (this.currentlist > this.lists.length - 1) {
        this.currentlist = this.lists.length - 1;
      }
      // this.todos.splice(index, 1);
    },

    removeTodo(itemID) {
      this.lists[this.currentlist].todos.forEach((todo, index) => {
        if (todo.id == itemID) {
          this.lists[this.currentlist].todos.splice(index, 1);
        }
      });
      // this.todos.splice(index, 1);
    },
    ChangeTodoCompletion(itemID) {
      let status = false;
      this.lists[this.currentlist].todos.forEach((todo, index) => {
        if (todo.id == itemID) {
          if (!this.lists[this.currentlist].todos[index].completed) {
            status = true;
          }
          this.lists[this.currentlist].todos[index].completed = status;
        }
      });
    },
    changeall() {
      this.lists[this.currentlist].todos.forEach(todo => {
        todo.completed = event.target.checked;
      });
    }
  },
  computed: {
    todosFiltered() {
      if (this.filter == "all") {
        return this.lists[this.currentlist].todos;
      } else if (this.filter == "completed") {
        return this.lists[this.currentlist].todos.filter(
          todo => todo.completed
        );
      } else if (this.filter == "notcompleted") {
        return this.lists[this.currentlist].todos.filter(
          todo => !todo.completed
        );
      }
      return this.lists[this.currentlist].todos;
    },
    remaining() {
      return this.lists[this.currentlist].todos.filter(todo => !todo.completed)
        .length;
    },
    noRemaining() {
      return this.remaining == 0;
    }
  }
};
</script>

<style scoped>
h4{
  margin: 0;
}
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
.list-title{
  margin: 1em 0 0 0;
}
.instructions{
  margin-bottom: 2em;
}
.todo-title {
  cursor: pointer;
  word-wrap: anywhere;
  margin-right: 1em;
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
.todolists {
  text-align: left;
  display: flex;
  flex-wrap: wrap;
}

.todo-list {
  position: relative;
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

.addlistbtn {
  border: none;
  border-radius: 50%;
}
.listaddinput {
  padding: 0 0 0.2em 0;
  border: none;
  border-bottom: 2px solid #4a4a4a;
}
.listremove {
  opacity: 0;
  position: absolute;
  top: -5%;
  right: 2%;
  border: none;
  padding: 0;
  color: red;
}
.listremove:hover {
  color: black;
}
.todo-list:first-of-type {
  margin-left: 1em;
}
.todo-list:hover .listremove {
  opacity: 1;
}

.hidden {
  width: 0;
  overflow: hidden;
  margin: 0;
}
</style>