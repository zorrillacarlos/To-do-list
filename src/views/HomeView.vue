<template>
  <div class="home container">
    <input
      type="text"
      class="form-control my-3"
      v-model="newTask"
      @keyup.enter="addTask"
    />
    <button class="btn btn-primary mx-3" @click="addTask">Agregar Tarea</button>
    <button class="btn btn-danger" @click="deleteAll">Borrar Tareas</button>

    <div class="mt-3" v-for="(item, index) in tasks" :key="index">
      <div
        role="alert"
        :class="['alert', item.state ? 'alert-success' : 'alert-danger']"
      >
        <div class="d-flex justify-content-between align-items-center">
          <div>{{ index + 1 }} - {{ item.name }} - {{ item.state }}</div>
          <div>
            <button
              class="btn btn-success btn-sm mx-2"
              @click="editTask(index)"
            >
              OK
            </button>
            <button class="btn btn-danger btn-sm" @click="deleteTask(index)">
              X
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HomeView",
  data() {
    return {
      tasks: [],
      newTask: "",
    };
  },
  methods: {
    addTask() {
      this.tasks.push({
        name: this.newTask,
        state: false,
      });
      this.newTask = "";
      localStorage.setItem("list-task", JSON.stringify(this.tasks));
    },
    editTask(index) {
      this.tasks[index].state = true;
      localStorage.setItem("list-task", JSON.stringify(this.tasks));
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
      localStorage.setItem("list-task", JSON.stringify(this.tasks));
    },
    deleteAll() {
      this.tasks = [];
      this.newTask = "";
      localStorage.clear();
    },
  },
  created() {
    let dataDB = JSON.parse(localStorage.getItem("list-task"));
    dataDB === null ? (this.tasks = []) : (this.tasks = dataDB);
  },
};
</script>

<style></style>
