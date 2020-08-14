<template>
  <v-card outlined dark>
    <v-list-item class="grad">
      <v-list-item-content>
        <div class="overline mb-4">Total</div>
        <v-list-item-title class="headline mb-1">Btl Jobs</v-list-item-title>
      </v-list-item-content>

      <v-list-item-avatar
        size="55"
        class="grey--text"
        color="white"
      >
        {{btl_count}}
      </v-list-item-avatar>
    </v-list-item>

  </v-card>
</template>



<script>

export default {


data: () => ({
btl_count:0
}),
async created () {  

    this.btl_count = await this.$axios.get('job').then(res => this.count = res.data.data.filter(v => v.job_type == 2).length);
},
methods : {

},
 
}

</script>