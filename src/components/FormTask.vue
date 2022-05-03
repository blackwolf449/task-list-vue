<template>
  <div class="taskDiv">
    <h1>New task</h1>
    <label>Name: <input type="text" v-model="taskName" /></label>
    <label>Description: <input type="text" v-model="taskDescription" /></label>
    <label>Data entrega: <input type="date" v-model="taskDate" /></label>
    <button class="btn btn-outline-dark" @dblclick="taskNew()">Create</button>
  </div>
</template>

<script>
class Task {
  constructor(id, taskName, taskDescription, taskDate) {
    this.id = id + 1;
    this.taskName = taskName;
    this.taskDescription = taskDescription;
    this.taskDate = taskDate;
    this.finished = false;
  }
}

import Controller from "./Controller.vue";
export default {
  name: "FormTask",
  components: { Controller },
  data() {
    return {
      taskName: "",
      taskDescription: "",
      taskDate: "",
    };
  },

  methods: {
    taskNew() {
      const taskArray = !localStorage.getItem("taskList")
        ? []
        : JSON.parse(localStorage.getItem("taskList"));
      const task = new Task(
        taskArray.length,
        this.taskName,
        this.taskDescription,
        this.taskDate
      );
      taskArray.push(task);
      localStorage.setItem("taskList", JSON.stringify(taskArray));
      console.log(taskArray);
    },
  },
};
</script>

<style scoped lang="scss">
.taskDiv {
  min-width: 200px;
  max-width: 280px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 10px;
  position: absolute;
  border-radius: 5px;
  border: 1px solid black;
  padding: 30px;
  background-color: white;
  left: 20%;
  right: 20%;
  margin: 0 auto;
  top: 70px;
}
.back-btn {
  display: flex;
  margin-right: auto;
  font-size: 15px;
  padding: 5px;
}
</style>