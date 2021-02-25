<template>
<div>
    <v-toolbar flat class="primary white--text">
        Lattest Jobs
    </v-toolbar>
  <v-data-table
    :headers="headers"
    :items="data"
    :items-per-page="5"
    class="elevation-2"
  >

  <template v-slot:item.status.keyword="{ item }">
    <v-chip dark small :class="status_class(item.status.id)">
      {{item.status.keyword}} 
    </v-chip>
    <v-chip v-if="item.status.id == 1" small color="secondary"> New </v-chip>

  </template>

  
  </v-data-table>
  

</div>
</template>

<script>
  export default {
    data () {
      return {
        headers: [
     
        {
          text: 'id',
          sortable: true,
          value: 'id',
        },

        
        {
          text: 'Job Title',
          sortable: true,
          value: 'task_title',
        },
        
        {
          text: 'District',
          sortable: true,
          value: 'district.district',
        },
        {
          text: 'Status',
          sortable: true,
          value: 'status.keyword',
        },

          {
          text: 'Created By',
          sortable: true,
          value: 'created_by_user.name',
        },
          {
          text: 'Assign to',
          sortable: true,
          value: 'assigned_to_user.name',
        },

          {
          text: 'Department',
          sortable: true,
          value: 'department.department',
        },
       

      ],
       data: [],
      }
    },

    created () {
        this.$axios.get('job').then(res => this.data = res.data.data);

        this.me = this.$auth.user;

      
    },
    methods : {
        status_class(val) {
  
        if(val == 1){
          return 'blue'
        }
        else if (val == 2){
          return 'success'
        }
        else if (val == 3){
          return 'orange'
        }
        else if (val == 8){
          return 'purple'
        }
         else if (val == 9){
          return 'secondary'
        }
        else{
          return 'red'
        }
      },
    }
  }
</script>