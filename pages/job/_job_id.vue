<template>
<v-row>
<v-col cols="7">
     <v-toolbar flat class="secondary mb-3" dark><strong>Job Details</strong>
     <v-spacer></v-spacer>
     <v-dialog v-model="dialog" persistent max-width="600px">
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="primary"
          dark
          v-bind="attrs"
          v-on="on"
        >
          Add Revision
        </v-btn>
      </template>
      <v-card>
         
          <v-container>
           
             
        <v-form
        ref="form"
        lazy-validation
        >
         <v-toolbar flat class="secondary mb-3" dark><strong>Revision</strong>
        <v-spacer></v-spacer>
               <v-btn class="primary" @click="onPick_attachment">
                  {{get_attachment_name}}
                 <v-icon> mdi-upload</v-icon>
                 </v-btn>   
                <input required type="file" @change="check_attachment" style="display:none;" accept="image/*" ref="attachmentInput">
        </v-toolbar>

        <v-row>
            <v-col cols="12">
            <v-textarea
            rows="3"
            class="px-5"
            v-model="msg"
            :rules="Rules"
            label="Item"
            required
            ></v-textarea>

            </v-col>
            <v-col cols="2">
                <v-btn
                color="secondary"
                class="mr-4 mx-5"
                @click="add_revision"
                >
                Save
                </v-btn>
            </v-col>
             <v-col cols="2">
                <v-btn
                color="primary"
                class="mr-4 mx-5"
                @click="dialog = false"
                >
                close
                </v-btn>
            </v-col>
        </v-row>
  </v-form>

          </v-container>
      </v-card>
    </v-dialog>
         </v-toolbar>
                  
     <v-simple-table>
    
    <template v-slot:default>
      <tbody>
      
         <tr>
          <th>Job</th>
          <td>{{ item.job_type == 1 ? 'Atl' : 'Btl'  }}</td>
        </tr>

          <tr>
          <th>Name</th>
          <td>{{ item.task_title }}</td>
        </tr>

         <tr>
          <th>Nature of Task</th>
          <td>{{ item.nature_of_task }}</td>
        </tr>

        <tr>
          <th>Brief</th>
          <td>{{ item.brief }}</td>
        </tr>

         <tr>
          <th>Deliverables</th>
          <td>{{ item.deliverables }}</td>
        </tr>

         <tr>
          <th>Attachment</th>
          <td>

            <v-dialog v-model="dialog1" max-width="900px">
            <template v-slot:activator="{ on }">
            <div class="pa-5">
                 <v-img v-on="on" height="175px" width="250px" :src="item.attachment"></v-img>
            </div>
            </template>

            <v-img height="auto" width="100%" :src="item.attachment"></v-img>
            </v-dialog> 

          </td>
        </tr>

         <tr>
          <th>From</th>
          <td>{{ item._from }}</td>
        </tr>

         <tr>
          <th>To</th>
          <td>{{ item._to }}</td>
        </tr>
         <tr>
          <th>Created By</th>
          <td>{{ created_by }}</td>
        </tr>

        <tr>
          <th>Assigned To</th>
          <td>{{ assigned_to }}</td>
        </tr>

        <tr v-if="department">
          <th>Department</th>
          <td>{{ department }}</td>
        </tr>

       <tr>
          <th>Status</th>
          <td>{{ status }}</td>
        </tr>

        <tr v-if="district">
          <th>District</th>
          <td>{{ district }}</td>
        </tr>

         <tr>
          <th>Submitted</th>
          <td>{{ item.created_at }}</td>
        </tr>

      </tbody>
    </template>
  </v-simple-table>

  </v-col>
  <v-col cols="1"></v-col>
  <v-col  cols="4" v-if="item.job_type == 1">
      <Chat :job_id="job_id" :r_id="r_id" :rev="revision" />  
  </v-col>
  </v-row>
</template>

<script>
import Chat from '@/components/Chat';
  export default {
      components : {
          Chat : Chat
      },
    data () {
      return {
          dialog:false,
          dialog1:false,
          headers:[
            {
                text: 'id',
                sortable: true,
                value: 'id',
            },
             {
                text: 's_id',
                sortable: true,
                value: 's_id',
            },
             {
                text: 'r_id',
                sortable: true,
                value: 'r_id',
            },
            {
                text: 'Revision',
                sortable: true,
                value: 'msg',
            },
            {
                text: 'Response At',
                sortable: true,
                value: 'created_at',
            },
          ],
          Rules : [
            v => !!v || 'This field is required',
          ],
        resultls: "{}",
        item: {},
        created_by:'',
        assigned_to:'',
        department:'',
        district:'',
        status:'',
        job_type:'',
        revision:'',

        job_id : this.$route.params.job_id,
        user_id : this.$auth.user.id,
        msg : '',
        attachment : '',
        r_id:'',
        
      }
    },
  
    async created () {            

await this.$axios.get(`job/${this.job_id}`).then(res => {

this.item = res.data.data;
this.created_by = res.data.data.created_by_user.name;
this.assigned_to = res.data.data.assigned_to_user.name;
this.department = res.data.data.department ? res.data.data.department.department : '' ;
this.district = res.data.data.district ? res.data.data.district.district : '';
this.status = res.data.data.status.status;            

this.r_id = this.user_id == this.item.created_by ? this.item.assigned_to :this.item.created_by

this.me = this.user_id == this.item.created_by  ? true : false

});

    },
    computed : {
         get_attachment_name(){
             return this.attachment.name ? this.attachment.name : 'Upload Attachment';
        },
    },
    methods : {

         onPick_attachment () { 
        this.$refs.attachmentInput.click() 
        },

        check_attachment(e) { 
        this.attachment = e.target.files[0] || ''; 
        },
       
        add_revision () {

        if(this.$refs.form.validate()){


            var revision = {
                job_id : this.job_id,
                s_id : this.user_id,
                r_id : this.user_id == this.item.created_by ? this.item.assigned_to : this.item.created_by,
                msg : this.msg
            }

            this.$axios.post(`revision`,revision).then(res => {

            this.revision = res.data.data;

            
            let payload = new FormData();
            payload.append('attachment',this.attachment);


            this.$axios.post('update_attachment/' + this.job_id, payload)
            .then(res => {
              this.item.attachment = res.data.attachment
              this.dialog = false
              this.$refs.form.reset()
            
            }).catch(error => console.log(error));
            
        });
         }

        }
    }

  }
</script>