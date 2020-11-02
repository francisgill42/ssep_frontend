<template>
<div>
<v-card style="max-height:500px" class="d-flex flex-column bordered" 
>

<v-toolbar flat class="primary mb-1" dark><strong>Revisions {{me.id == 2 ? '' : 'with PMU'  }} </strong>
<v-spacer></v-spacer>
<AddRevisionPMU v-if="item.status_id != 7"
  :revision_title="'Send Response'"
 :size="true" :btn_class="'secondary lighten-2'" :job_id="job_id" :item="item" />  

</v-toolbar>

<v-card-text class="flex-grow-1 overflow-y-auto">
    <template>
    <div v-for="(r, i) in revisions_with_user" :key="i"
    :class="{ 'd-flex flex-row-reverse': r.me }"
    >
    <v-menu offset-y>
      <template v-slot:activator="{ on }">
       
      <v-chip
      :color="r.me ? 'primary lighten-1' : 'secondary lighten-1'" dark style="height:auto;white-space: normal;"
      class="pa-4 mb-2"
      v-on="on"
      >
      
      <table>
        <tr> 
          <td colspan="2"><p style="font-size: 1rem; position:relative;">{{r.msg}}</p></td>
        </tr>
     
        <tr>
          <td>{{ r.name }}</td>
          <td class="text-right">{{ r.created_at }}</td>
        </tr>
      </table>
      </v-chip>

      </template>

  </v-menu>

  </div>
  </template>
    <p class="text-center">{{no_records}}</p>
</v-card-text>
</v-card>
</div>
</template>


<script>
import AddRevisionPMU from '@/components/AddRevisionPMU';

export default {

components : { AddRevisionPMU },

props: ['job_id','r_id','item'],
data: () => ({
admin:false,  
revisions_with_user : [],
msg : '',
holder:'',
no_records:'',
no_records_pmu:'',
me : {}

}),
async created () {  
  
    this.$nuxt.$on('revision_pmu', (v) => {
    

       var payload = {

        name : v.sender.name,
        msg : v.msg,
        me : v.s_id == this.$auth.user.id ? true : false,
        isMaster : v.sender.master ? true : false,
        created_at : v.created_at

      };
    
    this.revisions_with_user.push(payload);   
    this.no_records = '';
   })

 this.me = this.$auth.user;

 this.get_chat();


},
 watch: {
    rev: function (v) {

      var payload = {

        name : v.sender.name,
        msg : v.msg,
        me : v.s_id == this.$auth.user.id ? true : false,
        isMaster : v.sender.master ? true : false,
        created_at : v.created_at

      };
    
    this.revisions_with_user.push(payload);   
    this.no_records = '';
    },
  
  },
methods : {

      get_chat (slug) {
     
        var payload = { 
          s_id : this.$auth.user.id, 
          r_id : this.$auth.user.master ? this.item.created_by : 2 
          };
        console.log(payload);
        this.$axios.post('revisions/' + this.job_id , payload)
        .then(res => {
          
          res.data.data.map((v => {

                  this.revisions_with_user.push({
                  name : v.sender.name ,
                  msg : v.msg,
                  me : v.s_id == this.$auth.user.id ? true : false,
                  created_at : v.created_at
                  });
              }));

          this.no_records = res.data.data.length == 0 ? 'No revisions added yet'  : '';
});

       
  }
 
},
 
}

</script>