<template>
  <div class="home">
    <v-text-field
      label="Add Task"
      class="pa-5"
      append-icon="mdi-plus"
      v-model="newTask"
      @click:append="addNewTask"
      @keydown.enter="addNewTask"
      hide-details
      clearable
    ></v-text-field>
    <v-list flat class="pt-0">
      <div v-for="task in tasks" :key="task.id">
        <v-list-item
          @click="doneTask(task.id)"
          :class="{ 'blue lighten-5': task.done }"
        >
          <template v-slot:default>
            <v-list-item-action>
              <v-checkbox :input-value="task.done" color="primary"></v-checkbox>
            </v-list-item-action>

            <v-list-item-content
              :class="{ 'text-decoration-line-through': task.done }"
            >
              <v-list-item-title>{{ task.title }}</v-list-item-title>
            </v-list-item-content>
            <v-list-item-action>
              <v-btn icon @click.stop="openDialog(task)">
                <v-icon color="primary lighten-1">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </template>
        </v-list-item>
        <v-divider></v-divider>
      </div>
    </v-list>
    <delete-dialog
      :dialog="dialog"
      :task="taskDelete"
      @delete="onDeleteTask"
      @closeDlg="onCloseDlg"
    />
    <div v-if="tasks.length === 0" class="content">
      <v-icon class="text-h1" color="primary lighten-1">mdi-check</v-icon>
      <h1>No Task</h1>
    </div>
  </div>
</template>

<script>
import DeleteDialog from '@/components/DeleteDialog.vue';
export default {
  name: 'Home',
  components: {
    DeleteDialog,
  },
  data() {
    return {
      tasks: [
        // {
        //   id: 1,
        //   title: 'Eat bananas',
        //   done: false,
        // },
        // {
        //   id: 2,
        //   title: 'Do exercise',
        //   done: false,
        // },
        // {
        //   id: 3,
        //   title: 'Go swimming',
        //   done: false,
        // },
      ],
      newTask: '',
      taskDelete: {},
      dialog: false,
    };
  },
  methods: {
    doneTask(id) {
      let task = this.tasks.filter((task) => task.id === id)[0];
      task.done = !task.done;
    },
    deleteTask(id) {
      if (confirm('Do you want to delete this task?')) {
        this.tasks = this.tasks.filter((task) => task.id !== id);
      }
    },
    addNewTask() {
      if (this.newTask === '') return;
      let task = {
        id: Date.now(),
        title: this.newTask,
        done: false,
      };

      this.tasks.push(task);
      this.newTask = '';
    },
    openDialog(task) {
      this.dialog = true;
      this.taskDelete = task;
    },
    onDeleteTask(taskDelete) {
      this.tasks = this.tasks.filter((task) => task !== taskDelete);
      this.dialog = false;
    },
    onCloseDlg() {
      this.dialog = false;
    },
  },
};
</script>

<style scoped>
.content {
  position: absolute;
  left: 50%;
  top: 50%;
  -webkit-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
}
</style>
