<template>

<v-card style="max-height:500px" class="d-flex flex-column bordered secondary" 
>
<v-toolbar flat class="primary mb-1" dark><strong>Revisions</strong></v-toolbar>
<v-card-title>
</v-card-title>
<v-card-text class="flex-grow-1 overflow-y-auto">
    <template>
    <div v-for="(r, i) in revisions" :key="i"
    :class="{ 'd-flex flex-row-reverse': r.me }"
    >
    <v-menu offset-y>
      <template v-slot:activator="{ on }">
      <v-chip
      :color="r.me ? 'primary darken-1' : 'secondary darken-1'" dark style="height:auto;white-space: normal;"
      class="pa-4 mb-2"
      v-on="on"
      >
      {{r.name}}
      {{ r.msg }}

      <sub class="ml-2" style="font-size: 0.7rem;">{{ r.created_at }}</sub>
      </v-chip>
      <br/>

      </template>

  </v-menu>
  </div>
  </template>
</v-card-text>
</v-card>

</template>


<script>
export default {
props: ['job_id','r_id','rev'],
data: () => ({

revisions : [],
msg : '',
holder:''

}),
async created () {   


await this.$axios.post(`revision_by_user/${this.job_id}`,{
s_id : this.$auth.user.id,
r_id : this.r_id
})
.then(res => {
res.data.data.map((v => {

var payload = {
name : v.sender.name ,
msg : v.msg,
me : v.s_id == this.$auth.user.id ? true : false,
created_at : v.created_at
};
this.revisions.push(payload);                  
}));

});



},
 watch: {
    rev: function (v) {
     
      var payload = {

        name : v.sender.name,
        msg : v.msg,
        me : v.s_id == this.$auth.user.id ? true : false,
        created_at : v.created_at

      };
    
    this.revisions.push(payload);   
    },
  
  },
methods : {
 
}

}
</script>