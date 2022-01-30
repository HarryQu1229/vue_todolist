<template>
  <div class="todo-header">
    <input
      type="text"
      placeholder="Please enter your todo task and press enter"
      @keyup.enter="addTodo"
      v-model="input"
    />
  </div>
</template>

<script>
import { nanoid } from "nanoid";
export default {
  name: "TodoHeader",
  data() {
    return {
      input: "",
    };
  },
  methods: {
    addTodo(event) {
      if (this.input.trim() !== "") {
        //reset to default
        event.target.placeholder =
          "Please enter your todo task and press enter";
        event.target.className = "";
        // create new object for the new Todo
        const newTodo = {
          id: nanoid(),
          content: this.input.trim(),
          completed: false,
          isEdit: false,
        };

        // send to App.vue
        this.$emit("receive", newTodo);
        // clear input box
        this.input = "";
      } else {
        // show the error message
        event.target.placeholder =
          "cannot add an empty todo, please write something!";
        event.target.className = "wrong";
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/*header*/
.todo-header input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}
.todo-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(82, 168, 236, 0.6);
}

.todo-header .wrong:focus {
  border-color: rgba(195, 50, 37, 0.8) !important;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(116, 51, 25, 0.6) !important;
}
</style>
