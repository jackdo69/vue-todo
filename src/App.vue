<template>
  <div id="app" class="container">
    <h1 class="text-center">Todo App</h1>
    <form @submit.prevent="addTodo()">
      <div class="form-group">
        <label for="exampleInputEmail1">New Todo</label>
        <input
          v-model="newTodo"
          placeholder="Walk around the block...."
          type="text"
          class="form-control"
          id="newTodo"
          aria-describedby="newTodoHelp"
        />
        <small id="newTodoHelp" class="form-text text-muted"
          >Type the task in the box above</small
        >
      </div>

      <button type="submit" class="btn btn-primary">Add Todo</button>
    </form>
    <ul class="list-group mt-3">
      <li class="list-group-item" v-for="(todo, index) in todos">
        <button
          v-if="!todo.done"
          @click="markDone(todo)"
          class="btn btn-success"
        >
          Done
        </button>
        <button @click="remove(index)" type="button" class="btn btn-danger">
          Delete
        </button>
        <span
          :class="{
            isDone: todo.done
          }"
          >{{ todo.title }}</span
        >
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      newTodo: "",
      todos: []
    };
  },
  watch: {
    todos: {
      handler() {
        localStorage.todos = JSON.stringify(this.todos);
      },
      deep: true
    }
  },
  mounted() {
    if (localStorage.todos) {
      this.todos = JSON.parse(localStorage.todos);
    }
  },
  methods: {
    addTodo() {
      this.todos.push({
        title: this.newTodo,
        done: false
      });
      this.newTodo = "";
    },
    markDone(todo) {
      todo.done = true;
    },
    remove(index) {
      this.todos.splice(index, 1);
    }
  }
};
</script>

<style>
.isDone {
  text-decoration: line-through;
}
</style>
