<template>
    <v-app>
    <v-app-bar app expanded flat>
      <v-toolbar 
        dark
        prominent
        color="blue-darken-3"
      >
        <v-app-bar-nav-icon></v-app-bar-nav-icon>
        <v-toolbar-title class = "text-center">FRAMEWORKS</v-toolbar-title>

        <v-btn @click="dialog = true"
        prepend-icon="mdi-plus"
      >
        <template v-slot:append>
          <v-icon></v-icon>
        </template>

        ADD

        </v-btn>
      </v-toolbar>
      <v-dialog v-model="dialog" max-width="350" max-height="500">
        <v-card>
          <v-card-title class="headline"> + Add Task</v-card-title>
          <v-card-text>
            <v-form ref = "form">
              <v-text-field   v-model="title" :rules="rules" label="Title"
              :error-messages="title ? [] : ['Title is required']" :error="!title" color="red"></v-text-field>
              <v-text-field v-model = "description" label="Description"
              :error-messages="description ? [] : ['Description is required']" :error="!description" color="red"></v-text-field>
              <v-text-field v-model = "deadline" label="Deadline" prepend-icon="event" type="date"></v-text-field>
              <v-radio-group v-model = "priority" row inline>
                <v-radio label="High" value="High"></v-radio>
                <v-radio label="Medium" value="Medium"></v-radio>
                <v-radio label="Low" value="Low"></v-radio>
              </v-radio-group>
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-btn prepend-icon="mdi-plus" color="blue-darken-3" @click="submitForm" >Add</v-btn>
            <v-btn prepend-icon="mdi-cancel" color="error" @click="dialog = false">Cancel</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>

    </v-app-bar>
    <v-container 
    position = "absolute"
    height = "100px"
    >
    <div class="my-table-wrapper">

      <v-table
      fixed-header
      class = "my-table"
      position = "absolute"
      height = "300px" 
      > 
        <thead>
          <tr>
            <th class="text left">
              Title
            </th>
            <th class="text left">
              Description
            </th>
            <th class="text left">
              Deadline
            </th>
            <th class="text left">
              Priority
            </th>
            <th class="text left">
              Is Complete
            </th>
            <th class="text left">
              Action
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in tasks" :key="item.id">
          <td>{{ item.title }}</td>
          <td>{{ item.description }}</td>
          <td>{{ item.deadline }}</td>
          <td>{{ item.priority }}</td>
          <td>            
            <v-checkbox v-model="item.isComplete" label="Is Complete"></v-checkbox>
          </td>
          <td>
            <v-layout column>
              <v-btn prepend-icon="mdi-square-edit-outline" color="blue-darken-3" @click="dialog1 = true"> Update</v-btn>
              <v-dialog v-model="dialog1" max-width="350" max-height="500">
                <v-card>
                  <v-card-title class="headline"> <v-icon>mdi-square-edit-outline</v-icon> Edit Task</v-card-title>
                  <v-card-text>
                    <v-form ref = "form">
                      <v-text-field v-model = "description" label="Description"
                      :error-messages="description ? [] : ['Description is required']" :error="!description" color="red"></v-text-field>
                      <v-text-field v-model = "deadline" label="Deadline" prepend-icon="event" type="date"></v-text-field>
                      <v-radio-group v-model = "priority" row inline>
                        <v-radio label="High" value="High"></v-radio>
                        <v-radio label="Medium" value="Medium"></v-radio>
                        <v-radio label="Low" value="Low"></v-radio>
                      </v-radio-group>
                    </v-form>
                  </v-card-text>
                  <v-card-actions>
                    <v-btn prepend-icon="mdi-square-edit-outline" color="blue-darken-3" @click="updateRow" >Edit</v-btn>
                    <v-btn prepend-icon="mdi-cancel" color="error" @click="dialog1 = false">Cancel</v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
              <v-btn prepend-icon = "alpha-x-circle" color="error" @click="deleteRow(item.id)">Delete</v-btn>
            </v-layout>
          </td>
        </tr>
        </tbody>
      </v-table>
    </div>
    </v-container>
  </v-app>

</template>

<script>

export default {

  data() {
    return {
      task: '',
      tasks: [],
      title: '',
      description: '',
      date: '',
      priority: '',
      dialog: false,
      dialog1: false,
      successText: '',
      editedTask: null,
      editedIndex: null,

    }
  },

  methods: {

    submitForm() {
      if (!this.title || !this.description || !this.deadline || !this.priority) {
        return;
      }
      const isTitleExists = this.tasks.some(task => task.title === this.title);
      if (isTitleExists) {
        this.successText = '';
        return;
      }

      this.tasks.push({
            title: this.title,
            description: this.description,
            deadline: this.deadline,
            priority: this.priority,
            isComplete: false,
          });

      this.successText = 'Task added!'
      const newtask = {
            title: this.title,
            description: this.description,
            deadline: this.deadline,
            priority: this.priority,
            isComplete: ''
          }
      console.log(newtask)
      this.dialog = false
      this.$refs.form.reset()

      
    },
    editRow(index) {
      this.editedIndex = index
      this.editedTask = { ...this.tasks[index] }

    },
    updateRow() {
      if (!this.description || !this.deadline || !this.priority) {
        return;
      }
      this.tasks.splice(this.editedIndex, 1, this.editedTask)

      this.editedTask = null
      this.editedIndex = null

      this.dialog1 = false
      this.$refs.form.reset()

    },
    deleteRow(index) {
      this.tasks.splice(index, 1);
    },
    AddClick() {
      this.$toasted.success('Add clicked!');
    }
  }
}
</script>

<style scoped>
.my-table-wrapper {
  position: relative;
  top: 75px;
}
</style>


