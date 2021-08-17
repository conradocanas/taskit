<template>
  <v-app>
    <v-app-bar app color="red darken-2" dark>
      <v-container class="d-flex align-center">
        <h2>Task IT</h2>
        <v-spacer></v-spacer>
        Home
      </v-container>
    </v-app-bar>

    <v-main>
      <v-container>
        <v-row>
          <v-col cols="12" lg="4" md="4">
            <v-card class="sidebar">
              <v-card-title>
                <h3>Agregar nueva tarea</h3>
              </v-card-title>
              <v-card-text>
                <v-form>
                  <v-text-field label="Titulo de tarea" v-model="taskTitle">
                  </v-text-field>
                  <v-text-field
                    label="Descripcion tarea"
                    v-model="taskDescription"
                  >
                  </v-text-field>
                  <v-btn
                    @click="createNewTask()"
                    block
                    color="red darken-4"
                    dark
                    >Agregar tarea</v-btn
                  >
                </v-form>
              </v-card-text>
            </v-card>
          </v-col>
          <v-col>
            <v-row>
              <v-col lg="6" md="6" v-for="(task, i) in tasks" :key="i">
                <v-card>
                  <v-card-title>
                    <h3>{{ task.title }}</h3>
                  </v-card-title>
                  <v-card-text>
                    <p>{{ task.description }}</p>
                  </v-card-text>
                  <v-card-actions class="d-flex justify-end">
                    <v-btn @click="deleteTask(task)" x-small text>
                      <v-icon color="red" left size="16">mdi-delete</v-icon>
                      Eliminar
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
      </v-container>
    </v-main>

    <div v-if="notification" class="notification-alert">
      <v-alert type="success"> {{ notificationText }} </v-alert>
    </div>
  </v-app>
</template>

<script>
import { getAllTasks, createTask, deleteTask } from "./services/tasks";

export default {
  data() {
    return {
      tasks: [],
      notification: false,
      notificationText: "",
      taskTitle: "",
      taskDescription: ""
    };
  },
  mounted() {
    this.getAllTasks();
  },
  methods: {
    async getAllTasks() {
      this.tasks = await getAllTasks();
    },
    async createNewTask() {
      const res = await createTask({
        title: this.taskTitle,
        description: this.taskDescription
      });
      this.showNotification(res.message)
      this.getAllTasks();
    },
    async deleteTask(task) {
      const res = await deleteTask(task);
      this.showNotification(res.message)
      this.getAllTasks();
    },
    showNotification(text) {
      this.notification = true;
      this.notificationText = text;
      setTimeout(() => {
        this.notification = false;
      }, 3000);
    }
  }
};
</script>

<style lang="scss" scoped>
.notification-alert {
  position: fixed;
  bottom: 10%;
  left: 50%;
  transform: translate(-50%, 0%);
}
</style>
