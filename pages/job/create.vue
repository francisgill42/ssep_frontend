<template>
          <v-card flat outlined>
            <v-form ref="form" lazy-validation>
            <v-card-title>
              <span class="headline">New Job</span>
              <v-spacer></v-spacer>
               
                <v-btn :rules="Rules" class="secondary" @click="onPick_attachment">
                  {{get_attachment_name}}
                 <v-icon> mdi-upload</v-icon>
                 </v-btn>   
                <input required multiple type="file" @change="check_attachment" style="display:none;" accept="image/*" ref="attachmentInput">
            </v-card-title>

            <v-card-text>
                
              <v-container>
                
                <v-row>
                      
                    <v-col cols="4" sm="4" md="4">
                    <v-text-field :rules="Rules"  v-model="editedItem.task_title" label="Job Title"></v-text-field>
                    </v-col>

                    <v-col cols="4" sm="4" md="4">
                    <v-text-field :rules="Rules"  v-model="editedItem.nature_of_task" label="Nature of Job"></v-text-field>
                    </v-col>

                     <v-col cols="4" sm="4" md="4">
                          <v-select
                        @change="get_job_type(editedItem.job_type)"
                        :rules="Rules"  
                        v-model="editedItem.job_type"
                        :items="[{id:1,job_type:'Atl'},{id:2,job_type:'Btl'}]"
                        item-text="job_type"
                        item-value="id"
                        label="Job Type"
                        ></v-select>
                    </v-col>
                
                   <v-col v-if="editedItem.job_type == 1" cols="12" sm="12" md="12">
                    <v-select
                        :rules="Rules"  
                        v-model="editedItem.department_id"
                        :items="departments"
                        item-text="department"
                        item-value="id"
                        label="Department"
                        ></v-select>
                  </v-col>

                    <v-col v-if="editedItem.job_type == 2" cols="12" sm="12" md="12">
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
                    <v-col cols="12" sm="12" md="12">
                         <v-textarea rows="3" v-model="editedItem.brief" label="Brief"></v-textarea>
                    </v-col>
               
                  


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
                        <v-date-picker :min="editedItem._from" :max="getEndDate" color="primary" v-model="editedItem._from" no-title scrollable>
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
                        <v-date-picker :min="editedItem._from" :max="getEndDate" color="primary" v-model="editedItem._to" no-title scrollable>
                        <v-spacer></v-spacer>
                        <v-btn text color="primary" @click="menu1 = false">Cancel</v-btn>
                        <v-btn text color="primary" @click="$refs.menu1.save(editedItem._to)">OK</v-btn>
                        </v-date-picker>
                        </v-menu>
                    </v-col>

                    <v-col v-if="editedItem.job_type == 2" cols="6" sm="6" md="6">
                    <v-dialog
                      ref="dialog"
                      v-model="modal2"
                      :return-value.sync="editedItem.from_time"
                      persistent
                      width="290px"
                    >
                      <template v-slot:activator="{ on, attrs }">
                        <v-text-field
                          v-model="editedItem.from_time"
                          label="From Time"
                          readonly
                          v-bind="attrs"
                          v-on="on"
                        ></v-text-field>
                      </template>
                      <v-time-picker
                        v-if="modal2"
                        v-model="editedItem.from_time"
                        full-width
                      >
                        <v-spacer></v-spacer>
                        <v-btn
                          text
                          color="primary"
                          @click="modal2 = false"
                        >
                          Cancel
                        </v-btn>
                        <v-btn
                          text
                          color="primary"
                          @click="$refs.dialog.save(editedItem.from_time)"
                        >
                          OK
                        </v-btn>
                      </v-time-picker>
                    </v-dialog>
                  </v-col>

                  <v-col v-if="editedItem.job_type == 2" cols="6" sm="6" md="6">
                    <v-dialog
                      ref="dialog1"
                      v-model="modal3"
                      :return-value.sync="editedItem.to_time"
                      persistent
                      width="290px"
                    >
                      <template v-slot:activator="{ on, attrs }">
                        <v-text-field
                          v-model="editedItem.to_time"
                          label="To Time"
                          readonly
                          v-bind="attrs"
                          v-on="on"
                        ></v-text-field>
                      </template>
                      <v-time-picker :min="editedItem.from_time"
                        v-if="modal3"
                        v-model="editedItem.to_time"
                        full-width
                      >
                        <v-spacer></v-spacer>
                        <v-btn
                          text
                          color="primary"
                          @click="modal3 = false"
                        >
                          Cancel
                        </v-btn>
                        <v-btn
                          text
                          color="primary"
                          @click="$refs.dialog1.save(editedItem.to_time)"
                        >
                          OK
                        </v-btn>
                      </v-time-picker>
                    </v-dialog>
                  </v-col>
                  
                   <!-- <v-col cols="12" sm="12" md="12">
                    <v-select
                        :rules="Rules" 
                        v-model="editedItem.assigned_to"
                        :items="users"
                        item-text="name"
                        item-value="id"
                        label="Assign to"
                        ></v-select>
                  </v-col> -->

                  <v-col>
                    <v-btn class="primary" text @click="save" :loading="loading">Save</v-btn>
                  </v-col>

                  
               
                
                </v-row>
              </v-container>
            
            </v-card-text>
              </v-form>
          </v-card> 
</template>

<script>
  export default {

    data: () => ({
      
        modal2: false,
        modal3 : false,

      loading : false,
      menu: false,
      menu1: false,
      Rules : [ v => !!v || 'This field is required'],
      users:[],
      districts:[],
      departments:[],
      editedIndex: -1,
      editedItem: {
          from_time: null,
        to_time : null,    
      change_attachment:'',
       //assigned_to:'',   
       job_type:0,
       task_title:'',nature_of_task:'',brief:'',deliverables:'',district_id:'',created_by:'',department_id:'',attachments:'', 
       _from: '',
       _to: '',
      },

      defaultItem: {   
          from_time: null,
        to_time : null,
       job_type:0,
       assigned_to:'',    
       task_title:'',nature_of_task:'',brief:'',deliverables:'',district_id:'',created_by:'',department_id:'',attachments:'',_from:'',_to:''
      },

    }),

    computed: {
        
       get_attachment_name(){

          let attachments = this.editedItem.attachments;
          
          return attachments.length ? 'Total Files (' + attachments.length + ')' : 'Upload Attachment' ;
      },
       getEndDate() {
     var date =  new Date();
     var endDate = new Date(date.getFullYear(), date.getMonth() + 12, 10);
     return endDate.toISOString().slice(0,10)
    }
    },


    created () {

      this.initialize()
    },

    methods: {

      initialize () {


      this.me = this.$auth.user;
      var {id,master,role_id} = this.me;

      this.$axios.get(`user_2be_assigned/${id}/${master}/${role_id}`).then(res => this.users = res.data.data );

      this.$axios.get('district').then(res => this.districts = res.data);

      this.$axios.get('dept_ex_btl').then(res => this.departments = res.data);

      },

        get_job_type(v){
          v == 1 ? this.editedItem.district_id = '' : this.editedItem.department_id = ''
        },
        onPick_attachment () { 
        this.$refs.attachmentInput.click() 
        },

        check_attachment(e) {
           this.editedItem.attachments = e.target.files || '';
        },
  
      save () {

        // console.log(this.editedItem.attachments);

          
        let payload = new FormData();
            payload.append('job_type',this.editedItem.job_type);
            payload.append('task_title',this.editedItem.task_title);
            payload.append('nature_of_task',this.editedItem.nature_of_task);
            payload.append('deliverables',this.editedItem.deliverables);
            payload.append('district_id',this.editedItem.district_id);
            payload.append('created_by',this.$auth.user.id);
            payload.append('department_id',this.editedItem.department_id);

            for(var j = 0; j < this.editedItem.attachments.length; j++){
              payload.append(`attachment[${j}]`, this.editedItem.attachments[j]);  
            }  

            payload.append('from',this.editedItem._from);
            payload.append('to',this.editedItem._to);
            payload.append('from_time',this.editedItem.from_time);
            payload.append('to_time',this.editedItem.to_time);
            
            //payload.append('assigned_to',this.editedItem.assigned_to);
            payload.append('brief',this.editedItem.brief);

            if(this.$refs.form.validate()){

              // this.loading = true;
          
             this.$axios.post('job',payload).then((res) => {
               this.$router.push('/job/');
              //  this.loading = false;
             });
        }
     
      },
      
    },
  }
</script>