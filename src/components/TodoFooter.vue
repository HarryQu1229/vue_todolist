<template>
  <div class="todo-footer" v-show="totalNumberOfTodos">
    <label>
      <input type="checkbox" v-model="allChecked" />
    </label>
    <span>
      <span>Completed {{ numberOfCompleted }}</span> /
      {{ totalNumberOfTodos }}
    </span>
    <button class="btn btn-danger" @click="clearAll">
      Clear All Completed Todos
    </button>
  </div>
</template>

<script>
export default {
  name: "TodoFooter",
  computed: {
    // get the number of completed Todos
    numberOfCompleted() {
      return this.todos.reduce(
        (pre, current) => pre + (current.completed ? 1 : 0),
        0
      );
    },
    // get total number of todos
    totalNumberOfTodos() {
      return this.todos.length;
    },
    // check whether all boxes are checked
    allChecked: {
      get() {
        return (
          this.numberOfCompleted === this.totalNumberOfTodos &&
          this.totalNumberOfTodos > 0
        );
      },
      // check or discheck all box when this total box is checked
      set(value) {
        this.$emit("checkAll", value);
      },
    },
    // activate the clearAll events
  },
  methods: {
    clearAll() {
      this.$emit("clearAll");
    },
  },
  props: ["todos"],
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}
.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}
.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}
.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>
