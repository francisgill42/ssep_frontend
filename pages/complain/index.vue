<template>
  <v-data-table
    :headers="headers"
    :items="data"
    :search="search"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat color="white">
        <v-toolbar-title>Complains</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-text-field
        label="Search"
        hide-details v-model="search"></v-text-field>
         <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-dialog v-model="dialog" max-width="900px">
          <template v-slot:activator="{ on }">
            <v-btn small dark class="secondary lighten-2 mb-2" v-on="on">Add Complain</v-btn>
          </template>
          <v-card>


            <v-card-text>
              <v-container>
                <v-row>


                  <v-col cols="12" sm="12" md="12" class="pt-6">
                    <h1>New Complain</h1>
                    </v-col>

                  <v-col cols="6" sm="6" md="6">
                    <v-text-field v-model="editedItem.consumer_id" label="Consumer Id"></v-text-field>
                    <div style="color:red;" v-if="errors.consumer_id">{{errors.consumer_id[0]}}</div>
                  </v-col>

                   <v-col cols="6" sm="6" md="6">
                    <v-text-field v-model="editedItem.consumer_name" label="Name"></v-text-field>
                    <div style="color:red;" v-if="errors.consumer_name">{{errors.consumer_name[0]}}</div>
                  </v-col>


                   <v-col cols="6" sm="6" md="6">
                    <v-text-field v-model="editedItem.consumer_mobile" label="Mobile"></v-text-field>
                    <div style="color:red;" v-if="errors.consumer_mobile">{{errors.consumer_mobile[0]}}</div>
                  </v-col>

                   <v-col cols="6" sm="6" md="6">
                    <v-text-field v-model="editedItem.consumer_cnic" label="Cnic"></v-text-field>
                    <div style="color:red;" v-if="errors.consumer_cnic">{{errors.consumer_cnic[0]}}</div>
                  </v-col>

                   <v-col cols="6" sm="6" md="6">
                    <v-text-field v-model="editedItem.consumer_address" label="Address"></v-text-field>
                    <div style="color:red;" v-if="errors.consumer_address">{{errors.consumer_address[0]}}</div>
                  </v-col>

                  <v-col cols="6" sm="6" md="6">
                    <v-text-field v-model="editedItem.consumer_city" label="City"></v-text-field>
                    <div style="color:red;" v-if="errors.consumer_city">{{errors.consumer_city[0]}}</div>
                  </v-col>

                  <v-col cols="6" sm="6" md="6">
                    <v-text-field v-model="editedItem.consumer_distric" label="District"></v-text-field>
                    <div style="color:red;" v-if="errors.consumer_distric">{{errors.consumer_distric[0]}}</div>
                  </v-col>

                  <v-col cols="6" sm="6" md="6">
                    <v-text-field v-model="editedItem.consumer_additional_information" label="Additional Information"></v-text-field>
                    <div style="color:red;" v-if="errors.consumer_additional_information">{{errors.consumer_additional_information[0]}}</div>
                  </v-col>




                    <v-col cols="12" sm="12" md="12">
                    <v-autocomplete
                        v-model="editedItem.consumer_complain_type"
                        :items="complain_types"
                        item-text="ct"
                        item-value="ct"
                        label="Complain Type"
                        ></v-autocomplete>
                        <div style="color:red;" v-if="errors.consumer_complain_type">
                        The Complain Type field is required.
                        </div>
                  </v-col>







                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn small class="primary" text @click="close">Cancel</v-btn>
              <v-btn small class="secondary lighten-2" text @click="save">Save</v-btn>

            </v-card-actions>



          </v-card>

        </v-dialog>
      </v-toolbar>
    </template>

     <template v-slot:item.actions="{ item }">

      <!-- <v-icon
        small
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon> -->

      <v-autocomplete
                        v-model="consumer_status"
                        :items="status_type"
                        label="Status"
                        item-text="st"
                        item-value="st"
                        @change="change_status(item.id)"

                        ></v-autocomplete>
    </template>







    <template v-slot:no-data>
      <v-btn small color="primary" @click="initialize">Reset</v-btn>
    </template>
  </v-data-table>
</template>




<script>
  export default {
    data: () => ({
      dialog: false,
      isActive: true,
      search:'',
      headers: [


{
text: '#',
sortable: true,
value: 'id',
},

{
text: 'Consumer No',
sortable: true,
value: 'consumer_id',
},


{
text: 'Ticket Number',
sortable: true,
value: 'consumer_ticket_number',
},

{
text: 'Status',
sortable: true,
value: 'consumer_status',
},
{
text: 'Name',
sortable: true,
value: 'consumer_name',
},
{
text: 'Mobile',
sortable: true,
value: 'consumer_mobile',
},
{
text: 'Cnic',
sortable: true,
value: 'consumer_cnic',
},

{
text: 'Address',
sortable: true,
value: 'consumer_address',
},
{
text: 'City',
sortable: true,
value: 'consumer_city',
},
{
text: 'District',
sortable: true,
value: 'consumer_distric',
},

{
text: 'Complain Type',
sortable: true,
value: 'consumer_complain_type',
},
{
text: 'Additional Information',
sortable: true,
value: 'consumer_additional_information',
},


{
text: 'Created At',
sortable: true,
value: 'created_at',
},



{ text: 'Change Status', value: 'actions', sortable: false },

      ],
      data: [],

      complain_types : [
            {
              ct:'Hardware Complain'
            },
            {
              ct:'Other Complain'
            }
        ],

        status_type : [
            {
              st:'Resolved'
            },
            {
              st:'Rejected'
            }
        ],

      consumer_status:'',
      editedIndex: -1,
      editedItem: {

          id : '',



          user_id: 1,
          consumer_id: '',
          consumer_name: '',
          consumer_mobile: '',
          consumer_cnic: '',
          consumer_address: '',
          consumer_city: '',
          consumer_distric: '',
          consumer_complain_type: '',
          consumer_additional_information: '',
          consumer_ticket_number: 'consumer_ticket_numbe',




      },

      errors:[],
      Rules : [
          v => !!v || 'This field is required',
        ],

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

      this.$axios.get('complain').then(res => this.data = res.data);


      },

      change_status(id){

        var payload = {
            consumer_status : this.consumer_status
          };


          this.$axios.post(`complain/change_status/${id}` , payload).then(res => {
            console.log(res.data)
          });

      },


       deleteItem (item) {
        const index = this.data.indexOf(item)
         confirm('Are you sure you want to delete this item?') &&
         this.$axios.delete('complain/'+item.id)
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

      this.$axios.post('complain',this.editedItem)
              .then((res) => {
                  console.log(res);
                    if(res.data.success){
                      this.data.push(res.data.data)
                      this.close()
                      this.errors = []
                      }
                      else{
                        this.errors = res.data.errors
                        }
            })
            .catch(err => console.log(this.errors = err.response.data.errors));



      },

    },

  }
</script>
