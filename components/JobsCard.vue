<template>
  <v-card
    max-width="344"
    outlined
    dark
  >
    <v-list-item class="grad">
      <v-list-item-content>
        <div class="overline mb-4" >Total</div>
        <v-list-item-title class="headline mb-1">Jobs</v-list-item-title>
      </v-list-item-content>

      <v-list-item-avatar
        size="55"
        class="grey--text"
        color="white"
      >
        {{totalcount}}
      </v-list-item-avatar>
    </v-list-item>

  </v-card>
</template>



<script>

const gradients = [
    ['#222'],
    ['#42b3f4'],
    ['red', 'orange', 'yellow'],
    ['purple', 'violet'],
    ['#00c6ff', '#F0F', '#FF0'],
    ['#f72047', '#ffd200', '#1feaea'],
  ];

export default {


data: () => ({
totalcount:0,
gradient: gradients[5],
}),
async created () {  

    this.totalcount = await this.$axios.get('job').then(res => this.count = res.data.data.length);
},
methods : {

},
 
}

</script>
