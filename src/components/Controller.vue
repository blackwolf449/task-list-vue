<template>
  <nav class="navbar navbar-light bg-light">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">To do list</a>
      <div class="d-flex">
        <button class="btn btn-outline-dark" @click="formShow()">
          Create new task
        </button>
      </div>
    </div>
  </nav>
  <FormTask class="front" v-if="showForm == true" @dblclick="formShow()" />
  <div class="container">
    <div class="card" v-for="(task, index) in tasks" :key="index">
      <p class="taskId">#{{ task.id }}</p>
      <div class="principalTask">
        <button
          class="none"
          v-if="task.finished == false"
          @click="finishedTask(task.id)"
        >
          <input type="checkbox" />
        </button>
        <p v-if="task.finished == true" class="p-bold">
          Task: {{ task.taskName }}
        </p>
        <p v-else>Task: {{ task.taskName }}</p>
      </div>
      <hr />
      <p>Description: {{ task.taskDescription }}</p>
      <p>Final date: {{ task.taskDate }}</p>
    </div>
  </div>
</template>

<script>
import FormTask from "./FormTask.vue";
export default {
  name: "Controller",
  components: { FormTask },
  data() {
    return {
      tasks: JSON.parse(localStorage.getItem("taskList")),
      showForm: false,
    };
  },
  methods: {
    finishedTask(id) {
      const index = id - 1;
      const taskArray = !localStorage.getItem("taskList")
        ? []
        : JSON.parse(localStorage.getItem("taskList"));
      taskArray[index].finished = true;
      taskArray.sort((a) => {
        if (a.finished == true) {
          return 1;
        }
        if (a.finished == false) {
          return -1;
        }
        return 0;
      });
      localStorage.setItem("taskList", JSON.stringify(taskArray));
      location.reload();
    },
    formShow() {
      if (this.showForm == true) {
        this.showForm = false;
        location.reload();
        return;
      }
      this.showForm = true;
    },
  },
};
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 10px;
  margin-top: 10px;
}
.card {
  width: 300px;
  padding: 10px;
  margin: 0 auto;
  z-index: 0;
}
.principalTask {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
}
.principalTask > p {
  margin-bottom: 0 !important;
}

.taskId {
  font-size: 12px;
  background-color: rgb(180, 180, 180);
  font-weight: bold;
  border-radius: 5px;
}

.p-bold {
  text-decoration-line: line-through;
}

.btn-left {
  margin-left: 10px;
}
.none {
  background-color: transparent;
  border-color: transparent;
  padding: 0;
}
.front {
  z-index: 1;
}
</style>