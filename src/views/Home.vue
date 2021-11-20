<template>
  <div v-show="showAddTask">
    <AddTask @add-task="addTask" />
  </div>
  <Tasks
    @toggle-reminder="setReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>

<script>
import Tasks from "../components/Tasks.vue";
import AddTask from "../components/AddTask.vue";
import { http } from "../utils/utils";
export default {
  name: "Home",
  components: {
    AddTask,
    Tasks,
  },
  props: {
    showAddTask: Boolean,
  },
  data() {
    return {
      tasks: [],
    };
  },
  methods: {
    async addTask(task) {
      const { data } = await http.post("/tasks", task);
      this.tasks = [...this.tasks, data];
    },
    deleteTask(id) {
      this.$swal
        .fire({
          icon: "warning",
          title: "Delete",
          text: "Are you sure you want to delete this item",
          showCancelButton: true,
          cancelButtonText: "Cancel",
          confirmButtonText: "Delete",
          confirmButtonColor: "red",
        })
        .then((result) => {
          if (result.isConfirmed) {
            this.tasks = this.tasks.filter((task) => task.id !== id);
            http.delete(`/tasks/${id}`);
          }
        });
    },
    async setReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const updatedTask = { ...taskToToggle, reminder: !taskToToggle.reminder };
      const { data, status } = await http.put(`/tasks/${id}`, updatedTask);
      status === 200
        ? (this.tasks = this.tasks.map((task) =>
            task.id === id ? { ...task, reminder: data.reminder } : task
          ))
        : alert("Error updating task");
    },
    async fetchTasks() {
      const { data } = await http.get("/tasks");
      return data;
    },
    async fetchTask(id) {
      const { data } = await http.get(`/tasks/${id}`);
      return data;
    },
  },
  emits: ["delete-task"],

  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>