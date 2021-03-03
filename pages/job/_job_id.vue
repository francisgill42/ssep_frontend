<template>
<div>
<v-row>

<v-snackbar color="primary" v-model="snackbar" :top="'top'">
{{msg}}
<v-btn small text dark  @click="snackbar = false">X</v-btn>
</v-snackbar>

<v-col cols="12">
<v-switch v-if="(me.role_id == 3 || me.role_id == 4) && job_type && (item.status.id == 1 || item.status.id == 2 || item.status.id == 4)" v-model="sw" @change="start_working" hide-details color="secondary" label="Start Working" />
</v-col>
<v-col cols="12" v-if="(me.role_id == 1 || me.id == 2) && item.status_id == 9" class="text-right">
  <v-btn small @click="Complete_job(item)" color="success">Complete this job</v-btn>
</v-col>  

<v-col cols="7">

<v-toolbar  flat class="primary mb-3" dark><strong> Job Details </strong>
<v-spacer></v-spacer>
<AddRevision v-if="sw == true && (me.role_id == 2 || me.role_id == 3 || me.role_id == 4) && (item.status.id == 2 || item.status.id == 3)" 
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
      <v-dialog v-model="dialog1" persistent width="550px" >
      <template v-slot:activator="{ on }">
        <v-btn color="primary" dark v-on="on">Open Gallery</v-btn>
      </template>
        <v-card >
        <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn text @click="dialog1 = false">X</v-btn>
        </v-card-actions>
        <v-carousel hide-delimiters class="pa-3" height="300px">

        <v-carousel-item v-for="i in attachment" :key="i" :src="i"></v-carousel-item>
        </v-carousel>

        </v-card>
    </v-dialog>


        <!-- <v-dialog v-model="dialog1" max-width="900px">
        <template v-slot:activator="{ on }">
        <div class="pa-5">
              <v-img v-on="on" height="175px" width="250px" :src="attachment"></v-img>
        </div>
        </template>

        <v-img height="auto" width="100%" :src="attachment"></v-img>
        </v-dialog>  -->

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
<v-col  cols="4">

<!-- <v-row>

   <v-col
      v-for="(n, i) in attachment"
      :key="i"
      cols="4"
    >
      <v-img
        :src="`${n}`"
        :lazy-src="`${n}`"
        aspect-ratio="1"
        class="grey lighten-2"
      >
      <v-icon dark color="secondary" @click="downloadImg(n)">mdi-download</v-icon>

      <v-spacer></v-spacer>

      <v-icon dark color="error" @click="deleteImg(n)">mdi-delete</v-icon>

        <template v-slot:placeholder>
          <v-row
            class="fill-height ma-0"
            align="center"
            justify="center"
          >
            <v-progress-circular
              indeterminate
              color="grey lighten-5"
            ></v-progress-circular>
          </v-row>
        </template>
      </v-img>

    </v-col>
</v-row> -->

<Chat v-if="delay && !me.master  && me.role_id != 7" :job_id="job_id" :item="item" />

<PMUChat v-if="delay && me.master || me.role_id == 1"  class="mt-3" :job_id="job_id" :item="item"/>  


</v-col>
   
</v-row>



</div>
</template>

<script>
import PMUChat from '@/components/PMUChat';
import Chat from '@/components/Chat';
import AddRevision from '@/components/AddRevision';
import axios from 'axios';
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
  // attachment : '',

  attachment : []
  
}
},

async created () {
  
  setTimeout(() => this.delay = true,3000);

  this.me = this.$auth.user; 
  
  await this.get_data();

  await this.$nuxt.$on('update_attachment', v => this.attachment = v);

},


methods : {

      deleteImg(arg){
        this.attachment.splice(this.attachment.indexOf(arg),1);
      },

      downloadImg(url) {
      var payload = {
        url : url,
        method : 'GET',
        responseType : 'blob',
      };
      axios(payload)
      .then((res) => {

        var url  = res.config.url;
        var pos = url.lastIndexOf('.') + 1;
        var ext = url.substr(pos);
        var fileUrl = window.URL.createObjectURL(new Blob([res.data]));
        var fileLink  = document.createElement('a');
            fileLink.href = fileUrl;

        fileLink.setAttribute('download' ,`image.${ext}`);
        document.body.appendChild(fileLink);

        fileLink.click();
      });
       
    },
   
  get_data () {
    
    this.$axios.get(`job/${this.job_id}`).then(res => {
    this.item = res.data.data;
    this.created_by = res.data.data.created_by_user.name;
    this.assigned_to = res.data.data.assigned_to_user ? res.data.data.assigned_to_user.name : '';
    this.department = res.data.data.department ? res.data.data.department.department : '' ;
    this.district = res.data.data.district ? res.data.data.district.district : '';
    this.keyword = res.data.data.status.keyword;     
    this.job_type = this.item.job_type == 1 ? 'Atl' : 'Btl'
    this.attachment = res.data.data.attachment;
    this.sw = this.item.status.id == 2 || this.item.status.id == 3 || this.item.status.id == 9 || this.item.status.id == 10 ? true : false ;
    });
  },
  start_working () {
      this.$axios.post(`change_status/${this.job_id}`,{sw:this.sw}).then(res => {

         this.msg = this.sw ? 'Job has beed started now' : 'You hold the job';
         this.item.status.id = res.data.data.status_id;
         this.snackbar = true;

    });

  },
  Complete_job(item){
        this.$axios.get(`complete_job/${item.id}`).then(res => { 
          this.item = res.data.data;
          this.keyword = res.data.data.status.keyword;
        });
  },
 }

}
</script>