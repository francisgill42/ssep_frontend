<template>
  <v-data-table
    :headers="headers"
    :items="data"
    sort-by="role"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat color="white">
        <v-toolbar-title>Jobs</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="900px">
          

          <template v-slot:activator="{ on }">
            <v-btn color="primary" dark class="mb-2" v-on="on">Add Job</v-btn>
          </template>
          <v-card>
            <v-form ref="form" lazy-validation>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
              <v-spacer></v-spacer>
               
                <v-btn :rules="Rules" class="secondary" @click="onPick_attachment">
                  {{get_attachment_name}}
                 <v-icon> mdi-upload</v-icon>
                 </v-btn>   
                <input required type="file" @change="check_attachment" style="display:none;" accept="image/*" ref="attachmentInput">
            </v-card-title>

            <v-card-text>
                
              <v-container>
                <v-row>
                      
                    <v-col cols="6" sm="6" md="6">
                    <v-text-field :rules="Rules"  v-model="editedItem.task_title" label="Job Title"></v-text-field>
                    </v-col>

                    <v-col cols="6" sm="6" md="6">
                    <v-text-field :rules="Rules"  v-model="editedItem.nature_of_task" label="Nature of Job"></v-text-field>
                    </v-col>
                
                    <v-col cols="6" sm="6" md="6">
                    <v-select
                        :rules="Rules"  
                        v-model="editedItem.department_id"
                        :items="departments"
                        item-text="department"
                        item-value="id"
                        label="Department"
                        ></v-select>
                  </v-col>

                    <v-col cols="6" sm="6" md="6">
                    <v-select
                        :rules="Rules" 
                        v-model="editedItem.district_id"
                        :items="districts"
                        item-text="district"
                        item-value="id"
                        label="District"
                        ></v-select>
                  </v-col>

                    <v-col cols="12" sm="12" md="12">
                    <v-textarea :rules="Rules" rows="3" v-model="editedItem.deliverables" label="Deliverables"></v-textarea>
                    </v-col>

                    <!-- <v-col cols="6" sm="6" md="6">
                         <v-textarea rows="3" v-model="editedItem.brief" label="Brief"></v-textarea>
                    </v-col>
                -->
                  


                    <v-col cols="6" sm="6" md="6">
                        <v-menu
                        ref="menu"
                        v-model="menu"
                        :close-on-content-click="false"
                        :return-value.sync="editedItem._from"
                        transition="scale-transition"
                        offset-y
                        min-width="290px"
                        >
                        <template v-slot:activator="{ on, attrs }">
                        <v-text-field
                        v-model="editedItem._from"
                        label="Starting from"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                        ></v-text-field>
                        </template>
                        <v-date-picker color="primary" v-model="editedItem._from" no-title scrollable>
                        <v-spacer></v-spacer>
                        <v-btn text color="primary" @click="menu = false">Cancel</v-btn>
                        <v-btn text color="primary" @click="$refs.menu.save(editedItem._from)">OK</v-btn>
                        </v-date-picker>
                        </v-menu>
                    </v-col>

                     <v-col cols="6" sm="6" md="6">
                     <v-menu
                        ref="menu1"
                        v-model="menu1"
                        :close-on-content-click="false"
                        :return-value.sync="editedItem._to"
                        transition="scale-transition"
                        offset-y
                        min-width="290px"
                        >
                        <template v-slot:activator="{ on, attrs }">
                        <v-text-field
                        v-model="editedItem._to"
                        label="Ending"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                        ></v-text-field>
                        </template>
                        <v-date-picker color="primary" v-model="editedItem._to" no-title scrollable>
                        <v-spacer></v-spacer>
                        <v-btn text color="primary" @click="menu1 = false">Cancel</v-btn>
                        <v-btn text color="primary" @click="$refs.menu1.save(editedItem._to)">OK</v-btn>
                        </v-date-picker>
                        </v-menu>
                    </v-col>
                     
                   <v-col cols="12" sm="12" md="12">
                    <v-select
                        :rules="Rules" 
                        v-model="editedItem.assigned_to"
                        :items="users"
                        item-text="name"
                        item-value="id"
                        label="Assign to"
                        ></v-select>
                  </v-col>
                  <!-- <v-col cols="12" sm="12" md="12">
                    <v-textarea
                        :rules="Rules" 
                        v-model="editedItem.brief"
                        label="Special Note"
                        ></v-textarea>
                  </v-col> -->
                
                </v-row>
              </v-container>
            
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="save">Save</v-btn>
            </v-card-actions>
              </v-form>
          </v-card> 
           
        </v-dialog>
      </v-toolbar>
    </template>

    <template v-slot:item.attachment="{ item }">
         <v-dialog v-model="dialog1" max-width="900px">
          

          <template v-slot:activator="{ on }">
             <div class="pa-5">
              <v-btn @click="img_holder = item.attachment" color="primary"  class="mb-2" v-on="on"> Open Image</v-btn>
             </div>
          </template>
          
          <v-img height="auto" width="100%" :src="img_holder"></v-img>
        </v-dialog> 
     
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
      img_holder:'',
      dialog: false,
      dialog1: false,
      menu: false,
      menu1: false,
      headers: [
     
        {
          text: 'id',
          sortable: true,
          value: 'id',
        },

        
        {
          text: 'Job Title',
          sortable: true,
          value: 'task_title',
        },

        
        {
          text: 'Nature of Task',
          sortable: true,
          value: 'nature_of_task',
        },

        
        {
          text: 'District',
          sortable: true,
          value: 'district.district',
        },
        {
          text: 'Status',
          sortable: true,
          value: 'status.status',
        },

          {
          text: 'Created By',
          sortable: true,
          value: 'created_by_user.name',
        },
          {
          text: 'Assign to',
          sortable: true,
          value: 'assigned_to_user.name',
        },

          {
          text: 'Department',
          sortable: true,
          value: 'department.department',
        },
          {
          align:'center',
          text: 'Attachment',
          sortable: true,
          value: 'attachment',
        },
        {
          text: 'Start Date',
          sortable: true,
          value: '_from',
        },
        {
          text: 'End Date',
          sortable: true,
          value: '_to',
        },
        { text: 'Actions', value: 'actions', sortable: false },

      ],
      data: [],
      districts: [],
      users:[],
      departments: [],
      att:'',
      Rules : [
       v => !!v || 'This field is required',
     ],
      editedIndex: -1,
      editedItem: {    
      change_attachment:'',
       assigned_to:'',   
       task_title:'',nature_of_task:'',brief:'',deliverables:'',district_id:'',created_by:'',department_id:'',attachment:'', 
       _from: new Date().toISOString().substr(0, 10),
       _to: new Date().toISOString().substr(0, 10)
      },

      defaultItem: {   
       change_attachment:'',
       assigned_to:'',    
       task_title:'',nature_of_task:'',brief:'',deliverables:'',district_id:'',created_by:'',department_id:'',attachment:'',_from:'',_to:''
      },

    }),

    computed: {

            get_attachment_name(){
              var res = '';
              res = this.editedIndex === -1 ? 'Upload Attachment' : this.editedItem.attachment;
              return this.editedItem.attachment.name ? this.editedItem.attachment.name : res ;
      },


      formTitle () {
        return this.editedIndex === -1 ? 'New Job' : 'Edit Job'
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

      this.$axios.get('job').then(res => console.log(this.data = res.data.data));

      this.$axios.get('user').then(res => this.users = res.data.data.filter((v) => v.master != 1 && v.id != this.$auth.user.id));

      this.$axios.get('district').then(res => this.districts = res.data);

      this.$axios.get('department').then(res => this.departments = res.data);

      },

        onPick_attachment () { 
        this.$refs.attachmentInput.click() 
        },

        check_attachment(e) { 
        this.editedItem.attachment = e.target.files[0] || ''; 
        },

        onPick_attachment_change () { 
        this.$refs.attachmentInput_change.click() 
        },

        check_attachment_change(e) { 
        this.editedItem.change_attachment = e.target.files[0] || ''; 
        },

        


      editItem (item) {
        this.editedIndex = this.data.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      deleteItem (item) {
        const index = this.data.indexOf(item)
         confirm('Are you sure you want to delete this item?') && 
         this.$axios.delete('job/'+item.id)
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
          
        let payload = new FormData();
            payload.append('task_title',this.editedItem.task_title);
            payload.append('nature_of_task',this.editedItem.nature_of_task);
            payload.append('deliverables',this.editedItem.deliverables);
            payload.append('district_id',this.editedItem.district_id);
            payload.append('created_by',this.$auth.user.id);
            payload.append('department_id',this.editedItem.department_id);
            payload.append('attachment',this.editedItem.attachment);
            payload.append('from',this.editedItem._from);
            payload.append('to',this.editedItem._to);
            payload.append('assigned_to',this.editedItem.assigned_to);

//            payload.append('brief',this.editedItem.brief);

            if(this.$refs.form.validate()){
          
            if (this.editedIndex > -1) {


            this.$axios.post('update_job/' + this.editedItem.id, payload)
            .then(res => {
              // const index = this.data.findIndex(item => item.id == this.editedItem.id)
              // this.data.splice(index, 1);
              Object.assign(this.data[this.editedIndex], res.data.data)
              this.close()
              this.$refs.form.reset()
            
            }).catch(error => console.log(error));


        } else {
              this.$axios.post('job',payload)
              .then((res) => {
              this.data.unshift(res.data.data)
              this.close()     
              this.$refs.form.reset()
           });

            }
        }
     
      },
      
    },
  }
</script>