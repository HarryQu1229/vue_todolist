<template>
  <div>
    <div id="root">
      <div class="todo-container">
        <div class="todo-wrap">
          <TodoHeader @receive="receive"></TodoHeader>
          <TodoList :todos="todos"></TodoList>
          <TodoFooter
            :todos="todos"
            @checkAll="checkAll"
            @clearAll="clearAll"
          ></TodoFooter>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TodoHeader from "./components/TodoHeader.vue";
import TodoList from "./components/TodoList.vue";
import TodoFooter from "./components/TodoFooter.vue";

export default {
  name: "App",
  components: {
    TodoHeader,
    TodoList,
    TodoFooter,
  },
  data() {
    return {
      todos: JSON.parse(localStorage.getItem("todos")) || [],
    };
  },
  watch: {
    todos: {
      deep: true,
      handler(newValue) {
        localStorage.setItem("todos", JSON.stringify(newValue));
      },
    },
  },
  methods: {
    // receiving new Todo from TodoHeader and add it
    receive(newTodo) {
      this.todos.unshift(newTodo);
    },
    // check or uncheck a Todo by its id
    checkTodo(id) {
      this.todos.forEach((todo) => {
        if (todo.id === id) {
          todo.completed = !todo.completed;
        }
      });
    },
    // delete a Todo by its id
    deleteTodo(id) {
      this.todos = this.todos.filter((todo) => !(todo.id === id));
    },
    // check or discheck all Todos
    checkAll(allChecked) {
      this.todos.forEach((todo) => {
        todo.completed = allChecked;
      });
    },
    // clear all completed Todos
    clearAll() {
      this.todos = this.todos.filter((todo) => !todo.completed);
    },
    // change Todo isEdit property
    editTodo(id) {
      this.todos.forEach((todo) => {
        if (todo.id === id) {
          todo.isEdit = true;
        }
      });
    },
    // update Todo after edit
    updateTodo(id, updatedTodoContent) {
      this.todos.forEach((todo) => {
        if (todo.id === id) {
          todo.content = updatedTodoContent;
          todo.isEdit = false;
        }
      });
    },
  },
  beforeMount() {
    this.todos.forEach((todo) => {
      todo.isEdit = false;
    });
  },
  mounted() {
    this.$bus.$on("checkTodo", this.checkTodo);
    this.$bus.$on("deleteTodo", this.deleteTodo);
    this.$bus.$on("editTodo", this.editTodo);
    this.$bus.$on("updateTodo", this.updateTodo);
  },
  beforeDestroy() {
    this.$bus.$off("checkTodo");
    this.$bus.$off("deleteTodo");
    this.$bus.$on("editTodo", this.updateTodo);
    this.$bus.$on("updateTodo", this.updateTodo);
  },
};
</script>

<style>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}

.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.btn-edit {
  margin-right: 5px;
  color: #fff;
  background-color: #22a11e;
  border: 1px solid #1d881a;
}

.btn-edit:hover {
  color: #fff;
  background-color: #177e13;
}
</style>
