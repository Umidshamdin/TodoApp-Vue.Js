<template>
  <section id="inputSubmit">
    <div class="container">
      <div class="row mt-5">
        <div class="inputButton">
          <form @submit.prevent="addTodo">
            <input
              v-model="input_content"
              placeholder="Enter name"
              class="form-control"
              type="text"
            />
            <button class="btn btn-primary">Add</button>
          </form>
        </div>
      </div>
      <div class="row">
        <div class="col-4 selectSubmit">
          <p>Position seç:</p>
          <select @change="selected" name="" id="">
            <option value=""></option>
            <option value="Front End">Front End</option>
            <option value="Back End">Back End</option>
            <option value="Full Stack">Full Stack</option>
            <option value="Marketing">Marketing</option>
            <option value="Marketing">UX,UI Design</option>
          </select>
        </div>
      </div>
    </div>
  </section>

  <section id="tableSubmit">
    <div class="container mt-5">
      <div class="row">
        <div class="tables">
          <table class="table table-dark table-striped">
            <thead>
              <tr>
                <th scope="col">#</th>
                <th scope="col">Name</th>
                <th scope="col">Position</th>
                <th scope="col">Settings</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(todo, todoIndex) in todos" key="todoIndex">
                <th scope="row">{{ todoIndex + 1 }}</th>
                <td>{{ todo.content }}</td>
                <td>{{ todo.selectOption }}</td>
                <td>
                  <button class="btn btn-danger" @click="deleteTodo(todoIndex)">
                    Delete
                  </button>
                  <button style="margin-left: 7px" class="btn btn-warning">
                    <router-link
                      style="text-decoration: none; color: white"
                      :to="'/editView/' + todoIndex"
                      >Edit</router-link
                    >
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, computed, watch } from "vue";

const todos = ref([]);
var results;
const input_content = ref("");
const selected = (e) => {
  results = e.target.value;
};

const addTodo = () => {
  if (input_content.value.trim() === "" || results === undefined) {
    return;
  }
  todos.value.push({
    content: input_content.value,
    selectOption: results,
    done: false,
  });
};
const deleteTodo = (todoIndex) => {
  todos.value.splice(todoIndex, 1);
};
watch(
  todos,
  (val) => {
    localStorage.setItem("todos", JSON.stringify(val));
  },
  { deep: true }
);

onMounted(() => {
  todos.value = localStorage.getItem("todos")
    ? JSON.parse(localStorage.getItem("todos"))
    : [];
});
</script>

<style lang="scss" scoped>
@import "../../assets/sass/todolist.scss";
</style>
