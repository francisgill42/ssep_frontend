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
        <v-dialog v-model="dialog" max-width="900px">
          <template v-slot:activator="{ on }">
            <v-btn color="primary" dark class="mb-2" v-on="on">Add User</v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
               <v-spacer></v-spacer>
                <v-checkbox
                color="secondary"
                v-model="isActive"
                :label="`Active`"
                ></v-checkbox>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                
                    <v-col cols="4" sm="4" md="4">
                    <v-select
                        v-model="editedItem.role_id"
                        :items="roles"
                        item-text="role"
                        item-value="id"
                        label="Role"
                        ></v-select>
                  </v-col>


                    <v-col cols="4" sm="4" md="4">
                    <v-select
                        v-model="editedItem.department_id"
                        :items="departments"
                        item-text="department"
                        item-value="id"
                        label="Department"
                        ></v-select>
                  </v-col>

                    <v-col cols="4" sm="4" md="4">
                    <v-select
                        v-model="editedItem.district_id"
                        :items="districts"
                        item-text="district"
                        item-value="id"
                        label="District"
                        ></v-select>
                  </v-col>


                  <v-col cols="6" sm="6" md="6">
                    <v-text-field v-model="editedItem.name" label="name"></v-text-field>
                  </v-col>
                 
                  <v-col cols="6" sm="6" md="6">
                    <v-text-field v-model="editedItem.email" label="Email"></v-text-field>
                  </v-col>

                   <v-col v-if="editedIndex ==  -1" cols="12" sm="12" md="12">
                    <v-text-field type="password" v-model="editedItem.password" label="Password"></v-text-field>
                  </v-col>
                  
                
                  <v-col cols="6" sm="6" md="6">
                    <v-text-field v-model="editedItem.mobile_no" label="Mobile No"></v-text-field>
                  </v-col>
                  
                  <v-col cols="6" sm="6" md="6">
                    <v-text-field v-model="editedItem.cnic" label="CNIC"></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="save">Save</v-btn>
            </v-card-actions>

            <template v-if="editedIndex > -1">

            <v-divider></v-divider>

              
            <v-card-title>
              <span class="headline">Password Update</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="12" md="12">
                    <v-text-field type="password" v-model="editedItem.change_password" label="New Password"></v-text-field>
                  </v-col>

                  <v-col cols="12" sm="12" md="12">
                        <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
                        <v-btn color="blue darken-1" text @click="change_password">Save</v-btn>
                  </v-col>
                

            
                </v-row>
              </v-container>
            </v-card-text>


            </template>

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
      isActive: true,
      headers: [
        {
          text: 'id',
          sortable: true,
          value: 'id',
        },
        {
          text: 'Role',
          sortable: true,
          value: 'role.role',
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
          value: 'district.district',
        },
         {
          text: 'Department',
          sortable: false,
          value: 'department.department',
        },
        {
          text: 'Active',
          sortable: true,
          value: 'isActive',
        },
        { text: 'Actions', value: 'actions', sortable: false },

      ],
      data: [],
      statusses : [],
      districts : [],
      departments : [],
      roles : [],
      editedIndex: -1,
      editedItem: {
      role_id: "",
      department_id : "",
      name: "",
      email: "",
     
      mobile_no: "",
      cnic: "",
      district_id: "",
      change_password: ""
      },
      defaultItem: {
      role_id: "",
      department_id : "",
      name: "",
      email: "",
      mobile_no: "",
      cnic: "",
      district_id: "",
      change_password: ""
      },
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New User' : 'Edit User'
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

      this.$axios.get('user').then(res => this.data = res.data.data.filter(v => v.master == 0));

      this.$axios.get('role').then(res => this.roles = res.data);

      this.$axios.get('department').then(res => this.departments = res.data);

      this.$axios.get('district').then(res => this.districts = res.data);
      
      },

      editItem (item) {
        this.editedIndex = this.data.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      deleteItem (item) {
        const index = this.data.indexOf(item)
         confirm('Are you sure you want to delete this item?') && 
         this.$axios.delete('user/'+item.id)
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

      change_password(){

          this.$axios.post('change_password/'+this.editedItem.id,{change_password:this.editedItem.change_password})
              .then((res) => {
                if(res.data.success){
                  this.close()
                }

              });   
      },

      save () {

          var payload = {
              role_id : this.editedItem.role_id,
              department_id : this.editedItem.department_id,
              name : this.editedItem.name,
              email : this.editedItem.email,
              password: this.editedItem.password,
              mobile_no : this.editedItem.mobile_no,
              cnic : this.editedItem.cnic,
              district_id : this.editedItem.district_id,
              isActive : this.isActive ? 1 : 0,
          };
        if (this.editedIndex > -1) {
       //   Object.assign(this.data[this.editedIndex], this.editedItem)

            this.$axios.put('user/' + this.editedItem.id, payload)
            .then(res => {
            
              const index = this.data.findIndex(item => item.id == this.editedItem.id)
              this.data.splice(index, 1,res.data.data);
              this.close()
     
            })
            .catch(error => console.log(error));


        } else {

              this.$axios.post('user',payload).then((res) => {
                    this.data.push(res.data.data)
                    this.close()
            });
        }
     
      },
    },
  }
</script>