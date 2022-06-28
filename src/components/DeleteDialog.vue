<template>
  <v-dialog v-model="dialog" max-width="290">
    <v-card>
      <v-card-title class="text-h5"> Delete Task </v-card-title>

      <v-card-text>
        Do you want to delete task <strong>{{ task.title }}</strong
        >?</v-card-text
      >

      <v-card-actions>
        <v-spacer></v-spacer>

        <v-btn color="gray darken-1" text @click="closeDialog"> Close </v-btn>

        <v-btn color="red darken-1" text @click="deleteTask(task)">
          Delete
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  props: {
    dialog: Boolean,
    task: Object,
  },
  methods: {
    deleteTask(task) {
      this.$emit('delete', task);
    },
    closeDialog() {
      this.$emit('closeDlg');
    },
  },
};
</script>
