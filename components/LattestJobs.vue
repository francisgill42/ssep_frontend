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
  ></v-data-table>

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
    }
  }
</script>