<template>
  <v-layout column>

  <v-row>
      <v-col  cols="4">
          <v-card
            color="secondary"
            dark
          >
            <v-card-title class="headline">Doctors</v-card-title>
            <v-card-actions>
              <v-btn text>{{dl}}</v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
 
  
  </v-layout>
</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'

export default {
  components: {
    Logo,
    VuetifyLogo
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
