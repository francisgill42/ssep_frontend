<template>
  <v-card dark>
    <v-list-item class="grad">
      <v-list-item-content>
        <div class="overline mb-4">Total</div>
        <v-list-item-title class="headline mb-1">ATL Jobs</v-list-item-title>
      </v-list-item-content>

      <v-list-item-avatar
        size="80"
        class="grey--text"
        color="white"
      >
      {{count}}
      </v-list-item-avatar>
    </v-list-item>

  </v-card>
</template>



<script>

export default {


data: () => ({
count:''
}),
async created () {  

  await this.$axios.get('get_job_count/' + 1).then(res => {

       this.count = res.data < 10 ? '0' + res.data : res.data;

    });
},
methods : {

},
 
}

</script>