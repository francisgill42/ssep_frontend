<template>
<div>
  <v-row>
    <v-col> 
      <TotalJobsCard /> 
    </v-col>
    
    <v-col> 
      <BtlJob /> 
    </v-col>
    
    <v-col> 
      <AtlJob /> 
    </v-col>
  </v-row>

  <v-row> 
    <v-col>
      <LattestJobs />
    </v-col>
  </v-row>  
</div>
</template>

<script>

import TotalJobsCard from '~/components/JobsCard.vue'
import AtlJob from '~/components/AtlJob.vue'
import BtlJob from '~/components/BtlJob.vue'
import LattestJobs from '~/components/LattestJobs.vue'

export default {
  components: {
    TotalJobsCard,
    AtlJob,
    BtlJob,
    LattestJobs
  },
  data (){
    return {
      items: [],
    }
  },
  created(){

  //  this.$axios.get('doctors').then(res => {

  //         this.dl = res.data.data.length
  
  //       });


  }
}
</script>
