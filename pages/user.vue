<template>
  <v-data-table
    :headers="headers"
    :items="data"
    sort-by="user"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat color="white">
        <v-toolbar-title>Users</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on }">
            <v-btn color="primary" dark class="mb-2" v-on="on">New Item</v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="12" md="12">
                    <v-select
                        v-model="editedItem.role_id"
                        :items="roles"
                        item-text="role"
                        item-value="id"
                        label="Role"
                        ></v-select>
                  </v-col>
                  <v-col cols="6" sm="6" md="6">
                    <v-text-field v-model="editedItem.username" label="Username"></v-text-field>
                  </v-col>
                 
                  <v-col cols="6" sm="6" md="6">
                    <v-text-field v-model="editedItem.email" label="Email"></v-text-field>
                  </v-col>
                  
                  <v-col cols="6" sm="6" md="6">
                    <v-text-field v-model="editedItem.password" label="Password"></v-text-field>
                  </v-col>

                  <v-col cols="6" sm="6" md="6">
                    <v-text-field v-model="editedItem.name" label="Name"></v-text-field>
                  </v-col>

                  <v-col cols="6" sm="6" md="6">
                    <v-text-field v-model="editedItem.mobile_no" label="Mobile No"></v-text-field>
                  </v-col>
                  
                  <v-col cols="6" sm="6" md="6">
                    <v-text-field v-model="editedItem.city" label="City"></v-text-field>
                  </v-col>
                  <v-col cols="6" sm="6" md="6">
                    <v-select
                        v-model="editedItem.district_id"
                        :items="district"
                        item-text="district"
                        item-value="id"
                        label="District"
                        ></v-select>
                  </v-col>
                  <v-col cols="6" sm="6" md="6">
                    <v-select
                        v-model="editedItem.status_id"
                        :items="status"
                        item-text="status"
                        item-value="id"
                        label="Status"
                        ></v-select>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="save">Save</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        small
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize">Reset</v-btn>
    </template>
  </v-data-table>
</template>

<script>
  export default {
    data: () => ({
      dialog: false,
      roles: [],
      status: [],
      district: [],
      headers: [
        {
          text: 'id',
          sortable: true,
          value: 'id',
        },
        {
          text: 'Role',
          sortable: true,
          value: 'role',
        },
        {
          text: 'Name',
          sortable: true,
          value: 'name',
        },
        {
          text: 'Email',
          sortable: false,
          value: 'email',
        },
        {
          text: 'Mobile No',
          sortable: false,
          value: 'mobile_no',
        },
        {
          text: 'District',
          sortable: false,
          value: 'district',
        },
        {
          text: 'Status',
          sortable: true,
          value: 'status',
        },
        { text: 'Actions', value: 'actions', sortable: false },

      ],
      data: [],
      editedIndex: -1,
      editedItem: {
       role_id:'',
       username:'',
       email:'',
       password:'',
       name:'',
       mobile_no:'',
       city:'',
       district_id:'',
       status_id:'',
      },
      defaultItem: {
       role_id:'',
       username:'',
       email:'',
       password:'',
       name:'',
       mobile_no:'',
       city:'',
       district_id:'',
       status_id:'',
      },
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      },
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
    },

    created () {
      this.initialize()
    },

    methods: {
      initialize () {

      this.$axios.get('manage-users').then(res => {

          this.data = res.data.data
        console.log(res.data)  

        });

        this.$axios.get('user_roles').then(res => {

          this.roles = res.data
        });

        this.$axios.get('status').then(res => {
          
          this.status = res.data
        });
        this.$axios.get('district').then(res => {
          
          this.district = res.data
        });

      },

      editItem (item) {
        this.editedIndex = this.data.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      deleteItem (item) {
        const index = this.data.indexOf(item)
         confirm('Are you sure you want to delete this item?') && 
         this.$axios.delete('manage-users/'+item.id)
            .then((res) => {
     
              const index = this.data.indexOf(item)
              this.data.splice(index, 1)
            
            });
      },

      close () {
        this.dialog = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      save () {

          var payload = {
              role_id : this.editedItem.role_id,
              username : this.editedItem.username,
              email : this.editedItem.email,
              password : this.editedItem.password,
              name : this.editedItem.name,
              mobile_no : this.editedItem.mobile_no,
              city : this.editedItem.city,
              district_id : this.editedItem.district_id,
              status_id : this.editedItem.status_id,
          };
        if (this.editedIndex > -1) {
       //   Object.assign(this.data[this.editedIndex], this.editedItem)

            this.$axios.put('manage-users/' + this.editedItem.id, payload)
            .then(res => {
            
            const index = this.data.findIndex(item => item.id == this.editedItem.id)
            this.data.splice(index, 1,res.data.data);
    
              this.close()
     
            })
            .catch(error => console.log(err));


        } else {
          
              this.$axios.post('manage-users',payload)
              .then((res) => {
              this.data.push(res.data.data)
              console.log(res.data)
              this.close()
              
            
            });
        }
     
      },
    },
  }
</script>