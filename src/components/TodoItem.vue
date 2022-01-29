<template>
  <li>
    <label>
      <input
        type="checkbox"
        :checked="todo.completed"
        @change="changeHandle(todo.id)"
      />
      <span>{{ todo.content }}</span>
    </label>
    <button class="btn btn-danger" @click="deleteHandle(todo.id)">
      Delete
    </button>
  </li>
</template>

<script>
export default {
  name: "TodoItem",
  props: ["todo", "checkTodo", "deleteTodo"],
  methods: {
    // call the checkTodo method from App.vue to update Todo
    changeHandle(id) {
      this.checkTodo(id);
    },
    // call the deleteTodo method from App.vue to deleteTodo
    deleteHandle(id) {
      if (confirm("Are you sure you want to delete?")) {
        this.deleteTodo(id);
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label input {
  vertical-align: middle;
  margin-right: 13px;
  position: relative;
  top: -1px;
  cursor: pointer;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background-color: #ddd;
}

li:hover .btn-danger {
  display: block;
}
</style>
