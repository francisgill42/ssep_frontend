<template>

  <v-data-table
    :headers="headers"
    :items="data"
    sort-by="patient"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat color="white">
        <v-toolbar-title>Patients</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on }">
            <v-btn color="primary" dark class="mb-2" v-on="on">New Item   </v-btn>
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
                        v-model="editedItem.user_id"
                        :items="doctors"
                        item-text="name"
                        item-value="id"
                        label="Doctor"
                        ></v-select>
                  </v-col>
                  <v-col cols="12" sm="12" md="12">
                    <v-text-field v-model="editedItem.p_name" label="Name"></v-text-field>
                  </v-col>
                 
                  <v-col cols="12" sm="12" md="12">
                    <v-text-field v-model="editedItem.p_cnic" label="CNIC"></v-text-field>
                  </v-col>
                  
                  <v-col cols="12" sm="12" md="12">
                    <v-text-field v-model="editedItem.p_mobile_no" label="Mobile No"></v-text-field>
                  </v-col>
                  
                  <v-col cols="12" sm="12" md="12">
                    <v-text-field v-model="editedItem.p_address" label="Address"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="12" md="12">
                    <v-select
                        v-model="editedItem.status_id"
                        :items="status"
                        item-text="status"
                        item-value="id"
                        label="Status"
                        ></v-select>
                  </v-col>
                  <v-col cols="12" sm="12" md="12">
                    <v-select
                        @change="getOfficers(editedItem.district_id)"
                        v-model="editedItem.district_id"
                        :items="district"
                        item-text="district"
                        item-value="id"
                        label="District"
                        ></v-select>
                  </v-col>

                    <v-col cols="12" sm="12" md="12">
                      
                    <v-select
                        v-model="editedItem.officer_id"
                        :items="officers"
                        item-text="fo"
                        item-value="fo_id"
                        label="Officers"
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

     <template v-slot:item.followup="{ item }">
<v-card   :to="'/patient/' + item.id" :hover="false" flat>

 <v-icon class="mx-0 px-0" small>mdi-eye</v-icon>
</v-card>
   

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
      doctors: [],
      status: [],
      district: [],
      officers:[],
      headers: [
        {
          text: 'id',
          sortable: true,
          value: 'id',
        },
        {
          text: 'Doctor',
          sortable: true,
          value: 'doctor',
        },
        {
          text: 'Name',
          sortable: true,
          value: 'p_name',
        },
        {
          text: 'CNIC',
          sortable: false,
          value: 'p_cnic',
        },
        {
          text: 'Mobile No',
          sortable: false,
          value: 'p_mobile_no',
        },
        {
          text: 'Address',
          sortable: false,
          value: 'p_address',
        },
        {
          text: 'Status',
          sortable: true,
          value: 'status',
        },
        { text: 'follow', value: 'followup', sortable: false },
        { text: 'Actions', value: 'actions', sortable: false },

      ],
      data: [],
      editedIndex: -1,
      editedItem: {
       user_id:'',
       p_name:'',
       p_cnic:'',
       p_mobile_no:'',
       p_address:'',
       status_id:'',
       district_id:'',
       officer_id:'',
      },
      defaultItem: {
       user_id:'',
       p_name:'',
       p_cnic:'',
       p_mobile_no:'',
       p_address:'',
       status_id:'',
       district_id:'',
       officer_id:'',
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
      getOfficers(d_id){

         this.$axios.post(`officers_by_district/${d_id}`).then(res => {

          this.officers = res.data
        console.log(res.data)  

        });

      },
      initialize () {

      this.$axios.get('patient').then(res => {

          this.data = res.data
        console.log(res.data)  

        });

        this.$axios.get('doctors').then(res => {

          this.doctors = res.data.data
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
         this.$axios.delete('patient/'+item.id)
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
              user_id : this.editedItem.user_id,
              p_name : this.editedItem.p_name,
              p_cnic : this.editedItem.p_cnic,
              p_mobile_no : this.editedItem.p_mobile_no,
              p_address : this.editedItem.p_address,
              status_id : this.editedItem.status_id,
              district_id : this.editedItem.district_id,
              field_officer_id:this.editedItem.officer_id,
          };
        if (this.editedIndex > -1) {
       //   Object.assign(this.data[this.editedIndex], this.editedItem)

            this.$axios.put('patient/' + this.editedItem.id, payload)
            .then(res => {
            
            const index = this.data.findIndex(item => item.id == this.editedItem.id)
            this.data.splice(index, 1,res.data.data);
   
              this.close()
     
            })
            .catch(error => console.log(err));


        } else {
          
              this.$axios.post('patient',payload)
              .then((res) => {
              this.data.push(res.data.data)
              this.close()
              
            
            });
        }
     
      },
    },
  }
</script>
<style>
.theme--light.v-card{
  background-color:none;
}
</style>