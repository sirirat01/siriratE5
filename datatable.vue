<template>
  <v-data-table
    :headers="headers"
    :items="desserts"
    :sort-by="[{ key: 'id', order: 'asc' }]"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>รายชื่อสมาชิก</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ props }">
            <v-btn class="mb-2" color="primary" dark v-bind="props">
              เพิ่มข้อมูล
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="editedItem.username"
                      label="username"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="editedItem.password"
                      label="password"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="editedItem.email"
                      label="email"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue-darken-1" @click="close">
                Cancel
              </v-btn>
              <v-btn color="blue-darken-1" @click="save">
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5">คุณต้องการลบข้อมูลนี้หรือไม่?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue-darken-1" @click="closeDelete">Cancel</v-btn>
              <v-btn color="blue-darken-1" @click="deleteItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon class="me-2" size="small" @click="editItem(item)">
        mdi-pencil
      </v-icon>
      <v-icon size="small" @click="deleteItem(item)">mdi-delete</v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize">Reset</v-btn>
    </template>
  </v-data-table>
</template>

<script>
import axios from "axios"
export default {
  data: () => ({
    dialog: false,
    dialogDelete: false,
    headers: [
      { title: 'ID', key: 'id' },
      { title: 'Username', key: 'username' },
      { title: 'Password', key: 'password' },
      { title: 'Email', key: 'email' },
      { title: 'Actions', key: 'actions', sortable: false },
    ],
    desserts: [],
    editedIndex: -1,
    editedItem: {
      id: '',
      username: '',
      password: '',
      email: '',
    },
    defaultItem: {
      id: '',
      username: '',
      password: '',
      email: '',
    },
  }),

  computed: {
    formTitle () {
      return this.editedIndex === -1 ? 'เพิ่มข้อมูล' : 'แก้ไขข้อมูล'
    },
  },

  watch: {
    dialog (val) {
      val || this.close()
    },
    dialogDelete (val) {
      val || this.closeDelete()
    },
  },

  async created () {
    this.initialize()
    try {
      const response = await axios.get("http://localhost:7000/list")
      this.desserts = response.data.row
    } catch (error) {
      console.error("Error fetching data:", error)
    }
  },

  methods: {
    initialize () {
      this.desserts = []
    },

    editItem (item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem (item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },

    async deleteItemConfirm () {
      try {
        const response = await axios.delete(`http://localhost:7000/delete/${this.editedItem.id}`)
        console.log("Delete response:", response.data)
        if (response.data.success) {
          this.desserts.splice(this.editedIndex, 1)
        }
        this.closeDelete()
      } catch (error) {
        console.error("Error deleting item:", error)
      }
    },

    close () {
      this.dialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    closeDelete () {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    async save () {
      try {
        let response
        if (this.editedIndex > -1) {
          response = await axios.put(`http://localhost:7000/update/${this.editedItem.id}`, this.editedItem)
        } else {
          response = await axios.post("http://localhost:7000/insert", this.editedItem)
        }
        console.log("Save response:", response.data)
        if (this.editedIndex > -1) {
          Object.assign(this.desserts[this.editedIndex], this.editedItem)
        } else {
          this.desserts.push(this.editedItem)
        }
        this.close()
      } catch (error) {
        console.error("Error saving item:", error)
      }
    },
  },
}
</script>
