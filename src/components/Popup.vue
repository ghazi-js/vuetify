<template>
  <v-dialog max-width="600px" v-model="dialog">
    <v-btn slot="activator" class="success">Add New Project</v-btn>
    <v-card>
      <v-card-title>
        <h2>Add a new project</h2>
      </v-card-title>
      <v-card-text>
        <v-form class="px-3" ref="form">
          <v-text-field label="Title" v-model="title" prepend-icon="folder" :rules="inputRules"></v-text-field>
          <v-textarea label="Information" v-model="info" prepend-icon="edit" :rules="inputRules"></v-textarea>
          <v-menu>
            <v-text-field
              :value="formattedDate"
              label="Due date"
              slot="activator"
              :rules="inputRules"
              prepend-icon="date_range"
            ></v-text-field>
            <v-date-picker v-model="due"></v-date-picker>
          </v-menu>
          <v-spacer></v-spacer>
          <v-btn flat class="success mx-0 mt-3" @click="submit" :loading="loading">Add Project</v-btn>
        </v-form>
      </v-card-text>
    </v-card>
  </v-dialog>
</template>

<script>
import format from "date-fns/format";
import { setTimeout } from "timers";
export default {
  data: () => ({
    title: "",
    info: "",
    due: null,
    inputRules: [v => v.length >= 3 || "Minimum length is 3 charachter"],
    loading: false,
    dialog: false
  }),
  methods: {
    submit() {
      if (this.$refs.form.validate()) {
        this.loading = true;
        setTimeout(() => {
          console.log(this.title, this.info, this.due);
          this.loading = this.dialog = false;
          this.$emit('projectAdded');
        }, 3000);
      }
    }
  },
  computed: {
    formattedDate() {
      return this.due ? format(this.due, "Do MMM YYYY") : "";
    }
  }
};
</script>
