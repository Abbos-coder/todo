<template>
  <div>
    <h1 class="mt-4 text-center">Todo List</h1>
    <div class="todo">
      <div class="todo__body mb-10">
        <div class="mb-5">
          <v-form
            ref="add"
            v-model="add_valid"
            lazy-validation
            class="d-flex"
          >
            <v-text-field
              outlined
              clearable
              dense
              label="New todo"
              v-model="add_todo"
              :rules="[v => !!v || 'Todo content is required']"
              required
              @keydown.enter.prevent="addTodo"
            />
            <v-btn color="primary" class="ml-4" @click="addTodo">add todo</v-btn>
          </v-form>
        </div>
        <div class="todo__items" v-for="(todo, idx) in todos" :key="idx">
          <div class="todo__item d-flex align-center">
            <div class="todo__item--content">
              <v-checkbox v-model="todo.status" color="warning">
                <template v-slot:label>
                  <s v-if="todo.status"> {{ todo.text }} </s>
                  <div v-else> {{ todo.text }} </div>
                </template>
              </v-checkbox>
            </div>
            <v-spacer/>
            <v-btn icon class="mr-2" @click="getCurrentTodo(todo.text, idx)">
              <v-icon color="green">mdi-square-edit-outline</v-icon>
            </v-btn>
            <v-btn icon @click="deleteTodo(idx)">
              <v-icon color="red">mdi-trash-can-outline</v-icon>
            </v-btn>
          </div>
        </div>
      </div>
    </div>

    <v-dialog v-model="edit_dialog" max-width="700">
      <v-card>
        <v-card-title class="mb-5">Edit todo</v-card-title>
        <v-card-text>
          <v-form v-model="edit_valid" lazy-validation ref="edit">
            <v-text-field
              outlined
              dense
              clearable
              label="Edit todo"
              v-model="current_todo"
              :rules="[v => !!v || 'Todo content is required']"
              required
              @keydown.enter.prevent="saveTodo"
            />
            <div class="d-flex justify-center mt-5">
              <v-btn color="primary" rounded @click="saveTodo">save</v-btn>
            </div>
          </v-form>
        </v-card-text>
      </v-card>
    </v-dialog>

  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data: () => ({
    add_valid: true,
    todos: [
      {
        id: 1,
        text: 'khamidullaev Abbos',
        status: false
      },
      {
        id: 2,
        text: 'Support',
        status: false
      },
      {
        id: 3,
        text: 'Company',
        status: false
      },
    ],
    add_todo: '',
    edit_dialog: false,
    current_todo: '',
    current_id: null,
    edit_valid: true
  }),
  methods: {
    addTodo() {
      const add_valid = this.$refs.add.validate()
      const new_todo = {
        id: this.todos.length + 1,
        text: this.add_todo,
        status: false
      }
      if (add_valid) {
        this.todos.unshift(new_todo);
        this.add_todo = ''
        this.$refs.add.reset()
      }

    },
    getCurrentTodo(todo, idx) {
      this.edit_dialog = true
      this.current_todo = todo
      this.current_id = idx
    },
    saveTodo() {
      const save_valid = this.$refs.edit.validate()
      if (save_valid) {
        this.edit_dialog = false
        this.todos[this.current_id].text = this.current_todo
      }
    },
    deleteTodo(idx) {
      this.todos.splice(idx, 1)
      this.$refs.add.reset()
    }
  }

}
</script>

<style lang="scss" src="assets/main.scss" scoped/>
