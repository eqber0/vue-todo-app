<template>
  <div class="container">
    <div class="todo-list">
      <h3 class="text-center pb-3">Todo List Title</h3>
      <input
        type="text"
        name=""
        id=""
        class="w-100"
        placeholder="Type something to do..."
        @keydown.enter="saveTodo"
      />
      <hr />
      <ul v-if="todoList.length > 0" class="p-0">
        <li
          v-for="item in todoList"
          :key="item.id"
          class="mt-3 d-flex justify-content-between align-items-center"
        >
          <span>{{ item.text }}</span>
          <button @click="deleteTodo(item)" class="btn btn-danger">
            Delete
          </button>
        </li>
      </ul>
      <div v-else>There is nothing to do...</div>
      <hr />
      <small class="d-flex justify-content-end">
        {{ todoList.length }} item left.</small
      >
    </div>
  </div>
</template>

<script>
import axios from "axios"
export default {
  data() {
    return {
      todoList: [],
    }
  },
  mounted() {
    axios.get("http://localhost:3000/items").then((res) => {
      this.todoList = res.data
    })
  },
  methods: {
    saveTodo(e) {
      axios
        .post("http://localhost:3000/items", { text: e.target.value })
        .then((res) => {
          this.todoList.push(res.data)
          e.target.value = ""
        })
    },
    deleteTodo(item) {
      axios.delete(`http://localhost:3000/items/${item.id}`).then((res) => {
        console.log(res)
        this.todoList = this.todoList.filter((i) => i.id !== item.id)
      })
    },
  },
}
</script>

<style>
.todo-list {
  height: 100%;
  padding: 5rem;
  width: 40rem;
  margin: auto;
  background-color: #e1e1e1;
}
</style>
