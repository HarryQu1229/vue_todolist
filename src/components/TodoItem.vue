<template>
  <li @dblclick="editHandle(todo.id)">
    <label>
      <input
        type="checkbox"
        :checked="todo.completed"
        @change="changeHandle(todo.id)"
      />
      <span v-show="!todo.isEdit">{{ todo.content }}</span>
      <input
        type="text"
        v-show="todo.isEdit"
        :value="todo.content"
        @blur="blurHandle(todo.id, $event)"
        @keyup.enter="$event.target.blur()"
        ref="updateTextBox"
      />
    </label>
    <button class="btn btn-danger" @click="deleteHandle(todo.id)">
      Delete
    </button>
    <button
      v-show="!todo.isEdit"
      class="btn btn-edit"
      @click="editHandle(todo.id)"
    >
      Edit
    </button>
  </li>
</template>

<script>
export default {
  name: "TodoItem",
  props: ["todo"],
  methods: {
    // call the checkTodo method from App.vue to update Todo
    changeHandle(id) {
      this.$bus.$emit("checkTodo", id);
    },
    // call the deleteTodo method from App.vue to deleteTodo
    deleteHandle(id) {
      if (confirm("Are you sure you want to delete?")) {
        this.$bus.$emit("deleteTodo", id);
      }
    },
    // edit existing Todo
    editHandle(id) {
      this.$bus.$emit("editTodo", id);
      this.$nextTick(function () {
        this.$refs.updateTextBox.select();
      });
    },
    // update the Todo content after edit
    blurHandle(id, event) {
      if (!event.target.value.trim()) {
        event.target.placeholder = "cannot be empty!";
        event.target.select();
      } else {
        this.$bus.$emit("updateTodo", id, event.target.value.trim());
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

li:hover .btn-edit {
  display: block;
}
</style>
