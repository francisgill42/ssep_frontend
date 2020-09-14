<template>

<v-row>

<v-snackbar color="primary" v-model="snackbar" :top="'top'">
{{msg}}
<v-btn small text dark  @click="snackbar = false">X</v-btn>
</v-snackbar>

<v-col cols="12">
<v-switch v-if="me.role_id == 3 || me.role_id == 4" v-model="sw" @change="start_working" hide-details color="secondary" label="Start Working" />
</v-col>


<v-col cols="7">

<v-toolbar  flat class="primary mb-3" dark><strong> Job Details </strong>
<v-spacer></v-spacer>
<AddRevision v-if="sw == true && (me.role_id == 2 || me.role_id == 3 || me.role_id == 4) && item.job_type == 1" 
  :revision_title="'Add Revision'"
 :size="true" :btn_class="'secondary lighten-2'" :job_id="job_id" :item="item" />

</v-toolbar>
            
<v-simple-table>

  <template v-slot:default>
  <tbody>

    <tr>
      <th>Job</th>
      <td>{{ job_type  }}</td>
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
              <v-img v-on="on" height="175px" width="250px" :src="attachment"></v-img>
        </div>
        </template>

        <v-img height="auto" width="100%" :src="attachment"></v-img>
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
      <td>{{ keyword }}</td>
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
<Chat v-if="delay && !me.master  && me.role_id != 7" :job_id="job_id" :item="item" />


<PMUChat v-if="delay && me.master || me.role_id == 1"  class="mt-3" :job_id="job_id" :item="item"/>  
</v-col>
</v-row>
</template>

<script>
import PMUChat from '@/components/PMUChat';
import Chat from '@/components/Chat';
import AddRevision from '@/components/AddRevision';
export default {
components : { PMUChat:PMUChat,Chat:Chat,AddRevision:AddRevision },
data () {
return {
    sw:false,
    snackbar:false,
    master : false,
    dialog:false,
    dialog1:false,
    delay:false,
    headers:[
      { text: 'id', sortable: true, value: 'id' },
      { text: 's_id', sortable: true, value: 's_id' },
      { text: 'r_id', sortable: true, value: 'r_id' },
      { text: 'Revision', sortable: true, value: 'msg' },
      { text: 'Response At', sortable: true, value: 'created_at' }
    ],
    Rules : [
      v => !!v || 'This field is required',
    ],
  item: {},
  created_by:'',
  assigned_to:'',
  department:'',
  district:'',
  keyword:'',
  job_type:'',
  job_id : this.$route.params.job_id,
  msg : '',
  attachment : '',
  
}
},

async created () {
  
  setTimeout(() => this.delay = true,3000);

  this.me = this.$auth.user; 
  
  await this.get_data();

  await this.$nuxt.$on('update_attachment', v => this.attachment = v);

},


methods : {
  get_data () {
    this.$axios.get(`job/${this.job_id}`).then(res => {


    this.item = res.data.data;
    this.created_by = res.data.data.created_by_user.name;
    this.assigned_to = res.data.data.assigned_to_user.name;
    this.department = res.data.data.department ? res.data.data.department.department : '' ;
    this.district = res.data.data.district ? res.data.data.district.district : '';
    this.keyword = res.data.data.status.keyword;     
    this.job_type = this.item.job_type == 1 ? 'Atl' : 'Btl'
    this.attachment = res.data.data.attachment;
    this.sw = this.item.status.id == 1 || this.item.status.id == 4 ? false : true ;
    });
  },
  start_working () {
      this.$axios.post(`change_status/${this.job_id}`,{sw:this.sw}).then(res => {

         this.msg = this.sw ? 'Job has beed started now' : 'You hold the job';

         this.snackbar = true;

    });

  }
 }

}
</script>