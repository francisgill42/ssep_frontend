<template>
  <v-data-table
    :headers="headers"
    :items="data"
    :search="search"    
    class="elevation-1"
  >
  
    <template v-slot:top>
      <v-toolbar flat color="white">
        <v-toolbar-title>Jobs
          
        </v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
         <v-text-field
        v-model="search"
        label="Search"
        hide-details
      ></v-text-field>
       <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
            <v-btn small v-if="me.role_id == 1 || me.role_id == 2" to="/job/create" dark class="secondary lighten-2 mb-2">Add Job</v-btn>
          <v-dialog v-model="dialog" max-width="900px">
          <v-card>
            <v-form ref="form" lazy-validation>
            <v-card-title>
              <span class="headline">Edit Job</span>
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




                     
                   <v-col v-if="editedItem.status_id == 1 || editedItem.status_id == 6" cols="12" sm="12" md="12">
                    <v-select
                        :rules="Rules" 
                        v-model="editedItem.assigned_to"
                        :items="users"
                        item-text="name"
                        item-value="id"
                        label="Assign to"
                        ></v-select>
                  </v-col>
                </v-row>
              </v-container>
            
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn class="primary" text @click="close">Cancel</v-btn>
              <v-btn class="secondary lighten-1" text @click="save">Save</v-btn>
            </v-card-actions>
              </v-form>
          </v-card> 
           
        </v-dialog>

          <v-dialog v-model="add_more_dialog" max-width="450px">
            <v-form ref="add_attachment_form"> 

          <v-card class="text-center pt-12" min-height="450px">
               <v-container>
                 <v-col>
                <v-btn fab color=" primary mt-16" class="white--text" @click="onPick_add_attachment">
                <input required multiple type="file" @change="check_add_attachment" style="display:none;" accept="image/*" ref="add_attachmentInput">        
                <v-icon dark>mdi-cloud-upload</v-icon>
                </v-btn>

                 </v-col>
                 <v-col>
                   Add More Attachments
                 </v-col>
                 <v-btn small class="secondary" @click="onPick_add_attachment_submit">
                  Submit
                </v-btn>
               </v-container>
          </v-card>
        
            </v-form>
          
        </v-dialog>

        <v-dialog v-model="dialog1" max-width="500px">
          <v-card>
            <v-form ref="form" lazy-validation>
          
            <v-card-text>
                
              <v-container>
                <v-row>
                    <v-col>
                    <v-select
                        :rules="Rules"  
                        v-model="editedItem.admin"
                        :items="admins"
                        item-text="name"
                        item-value="id"
                        label="Admin"
                        ></v-select>
                  </v-col>

                </v-row>
              </v-container>
            
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
                 <v-btn class="primary" text @click="dialog1 = false">Cancel</v-btn>
              <v-btn class="secondary lighten-1" text @click="share">Save</v-btn>
            </v-card-actions>
              </v-form>
          </v-card> 
           
        </v-dialog>
      </v-toolbar>
    </template>

  
    <template v-slot:no-data>
      <v-btn small color="primary" @click="initialize">Reset</v-btn>
    </template>

     <template v-slot:item.status.keyword="{ item }">
    <v-chip  dark small :class="status_class(item.status.id)">
      {{item.status.keyword}}
      </v-chip>
      <v-chip v-if="item.status.id == 1" small color="secondary"> New </v-chip>
  </template>

  <template v-slot:item.assigned_to_user.name="{ item }">
     <span v-if="item.assigned_to">{{item.assigned_to_user.name}}</span>
     <strong v-else>Not Assigned</strong>
  </template>

<template v-if="me.role_id && me.role_id == 4" v-slot:item.AR="{ item }">
  <div class="text-center">
    <div v-if="item.status.id == 8">
      <v-btn x-small class="ma-2" color="primary" dark @click="Approve(item)">Approve
        <v-icon dark right>mdi-checkbox-marked-circle</v-icon>
      </v-btn>

      <v-btn x-small class="ma-2" color="red" dark @click="Reject(item)">Reject
        <v-icon dark right>mdi-cancel</v-icon>
      </v-btn>
    </div>    
  </div>
</template>

        <template v-slot:item.actions="{ item }">
      
        <!-- <v-icon
        v-if="(me.role_id == 1 || me.role_id == 2) && item.status_id == 3"
        small
        class="mr-2"
        @click="shareItem(item)"
        > 
        mdi-rotate-right-variant
        </v-icon> -->

       <v-icon
        v-if="(me.role_id == 1 || me.role_id == 2) && item.status_id == 3"
        small
        class="mr-2"
        @click="multiple_box(item.id)"
        > 
        mdi-image-multiple
        </v-icon>
        

         <!-- <v-icon
        v-if="me.id == 2"
        small
        class="mr-2"
        @click="shareItem(item)"
        >
        mdi-share-variant
        </v-icon> -->

   <!-- 1.	Team Head ===> 7
            create, view, edit, share, delete and view logs

            2.	Team Sub-Head ===> 1

            create, view, edit, share,

            3.	User ==> 2
            view, edit and share

            4. Sub-Users ===> 3
            view, edit -->
        <v-icon
        small
        class="mr-2"
        @click="viewItem(item)"
        >
        mdi-eye
        </v-icon>

        <v-icon
        v-if="me.role_id == 1 || me.role_id == 2"
        small
        class="mr-2"
        @click="editItem(item)"
        >
        mdi-pencil
        </v-icon>

        <v-icon
        v-if="me.role_id == 1"
        small
        @click="deleteItem(item)"
        >
        mdi-delete
        </v-icon>
        </template>
  </v-data-table>
</template>

<script>
  export default {
    data: () => ({
      add_more_dialog: false,
      modal2: false,
      modal3 : false,
      search:'',
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
          text: 'District',
          sortable: true,
          value: 'district.district',
          
        },
        {
          text: 'Status',
          sortable: true,
          value: 'status.keyword',
          
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
          text: 'Created At',
          sortable: true,
          value: 'created_at',
          
        },
        

      ],
      admins:[],
      data: [],
      districts: [],
      users:[],
      departments: [],
      att:'',
      me:{},
      Rules : [
       v => !!v || 'This field is required',
     ],
      editedIndex: -1,
      editedItem: { 
       from_time: null,
       to_time : null,  
       assigned_to:'',
       admin:'',
       job_type:0,   
       task_title:'',nature_of_task:'',brief:'',deliverables:'',district_id:'',created_by:'',department_id:'',attachment:'', 
       _from: new Date().toISOString().substr(0, 10),
       _to: new Date().toISOString().substr(0, 10)
      },
      add_attachment : '',

    }),

    computed: {

     
        get_attachment_name(){

        return `Total Files (${this.editedItem.attachment.length})`;

        },
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
    },

    created () {
        this.initialize()

        this.setCustomHeader()

    },


    methods: {

        setCustomHeader(){
          
            if(this.me.role_id && this.me.role_id == 4){
              this.headers.push({sortable:true,text:'Approve/Reject',value:'AR'});
            }
          this.headers.push({sortable:true,text:'Actions',value:'actions'})

        },


       status_class(val) {
  
        if(val == 1){
          return 'blue'
        }
        else if (val == 2){
          return 'success'
        }
        else if (val == 3){
          return 'orange'
        }
        else if (val == 9){
          return 'purple'
        }
         else if (val == 10){
          return 'secondary'
        }
        else if (val == 8){
          return 'green'
        }
        else if (val == 6){
          return 'yellow'
        }
        else{
          return 'red'
        }
      },

      share () {

          this.$axios.post('share/' + this.editedItem.id,{user_id : this.editedItem.admin})
            .then(res => {

              Object.assign(this.data[this.editedIndex], res.data.data)
              this.dialog1 = false;
              this.$refs.form.reset()
            
            }).catch(error => console.log(error));

      },

      Approve(item){
        this.$axios.get(`approve_reject/${item.id}/a`).then(res => {

          item.status.id = res.data.data.status.id;
          item.status.keyword = res.data.data.status.keyword;
        });
      },
      Reject(item){

         const index = this.data.indexOf(item)
         confirm('Are you sure you want to reject this item?') && 
         this.$axios.get('approve_reject/'+item.id + '/r')
            .then((res) => {
              const index = this.data.indexOf(item)
              this.data.splice(index, 1)
            
            });

      },

      initialize () {

      this.me = this.$auth.user;
      var {id,master,role,role_id} = this.me;
      console.log(role_id);

      var job_slug = '';

      if(role){
          if(role_id == 4){
          job_slug = 'jobs_for_pmu'
          }
          else if(role_id == 2 || role_id == 3){ 
          job_slug = 'jobs_by_created_and_assigned/' + id;
          }
          
      }
      
      else{ job_slug = 'job' }

      this.$axios.get(job_slug).then(res => {
        console.log(this.data = res.data.data,job_slug);
      });

      this.$axios.get(`user_2be_assigned/${id}/${master}/${role_id}`).then(res => this.users = res.data.data );

      this.$axios.get('admins').then(res => this.admins = res.data.data.filter((v) => v.id != id ));
      
      this.$axios.get('district').then(res => this.districts = res.data);

      this.$axios.get('department').then(res => this.departments = res.data);

      },

        onPick_attachment () { 
          this.$refs.attachmentInput.click() 
        },

        check_attachment(e) { 
          this.editedItem.attachment = e.target.files || ''; 
        },
        check_add_attachment (e) {
          this.add_attachment = e.target.files || ''; 
        },
         onPick_add_attachment () { 
         this.$refs.add_attachmentInput.click(); 
        },
       
        shareItem (item) {
           this.editedIndex = this.data.indexOf(item)
           this.editedItem = Object.assign({}, item)
           this.dialog1 = true;
        },

       viewItem (item) {
        this.$router.push(`/job/${item.id}/`);
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
      close () { this.dialog = false },
      close_add_attachment () { this.add_more_dialog = false },
      multiple_box(v){

        this.editedItem.id = v;
        this.add_more_dialog = true
      },

       onPick_add_attachment_submit(){
        
            if(this.$refs.add_attachment_form.validate()){

              var payload = new FormData();

              for(var j = 0; j < this.add_attachment.length; j++){
                  payload.append(`attachment[${j}]`, this.add_attachment[j]);  
                } 

            this.$axios.post('add_attachment/' + this.editedItem.id, payload)
            .then(res => {

            this.close_add_attachment()
            this.$refs.add_attachment_form.reset()

            }).catch(error => console.log(error));

            }
        },

      save () {
          
        var payload = new FormData();
            payload.append('job_type',this.editedItem.job_type);
            payload.append('task_title',this.editedItem.task_title);
            payload.append('nature_of_task',this.editedItem.nature_of_task);
            payload.append('deliverables',this.editedItem.deliverables);
            payload.append('district_id',this.editedItem.district_id);
            payload.append('created_by',this.$auth.user.id);
            payload.append('department_id',this.editedItem.department_id);
           
            for(var j = 0; j < this.editedItem.attachment.length; j++){
              payload.append(`attachment[${j}]`, this.editedItem.attachment[j]);  
            }  
            payload.append('from',this.editedItem._from);
            payload.append('to',this.editedItem._to);
            payload.append('from_time',this.editedItem.from_time);
            payload.append('to_time',this.editedItem.to_time);
            payload.append('assigned_to',this.editedItem.assigned_to);
            payload.append('brief',this.editedItem.brief);

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