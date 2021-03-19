<template>
  <v-data-table
    :headers="headers"
    :items="data"
    :search="search"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat color="white">
        <v-toolbar-title>Activity List</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>

        <v-text-field
        v-model="search"
        label="Search"
        hide-details
      ></v-text-field>

       <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
       
      </v-toolbar>
    </template>

     <template v-slot:item.action="{ item }">
      <v-icon small @click="go_to_single(item)"
        
      >
        mdi-eye {{item.id}}
      </v-icon>
    </template>
   
    <template v-slot:no-data>
      <v-btn small color="primary" @click="initialize">Reset</v-btn>
    </template>


  </v-data-table>
</template>

<script>
  export default {
    data: () => ({
      dialog: false,
      search:'',
      headers: [
        
      
         {
          text: '#',
          sortable: true,
          value: 'id',
        },

         {
          text: 'Activity Name',
          sortable: true,
          value: 'activity_name',
        },

       {
          text: 'District',
          sortable: true,
          value: 'district',
        },

          


        
           {
          text: 'Village',
          sortable: true,
          value: 'village',
        },


        
           {
          text: 'Distric Officer Name',
          sortable: true,
          value: 'DO_name',
        },


      

         {
          text: 'No of Male',
          sortable: true,
          value: 'male',
        },

       {
          text: 'No of Female',
          sortable: true,
          value: 'female',
        },
      
          
        {
          text: 'Location',
          sortable: true,
          value: 'map_location',
        },

        
           {
          text: 'Submitted',
          sortable: true,
          value: 'date',
        },


        { text: 'Actions', value: 'action', sortable: false },
      ],
      data: [],
      editedIndex: -1,
    
    }),

  
    created () {
      this.initialize()
    },

    methods: {
      initialize () {

        this.$axios.get('fieldactivity').then(res => {
          this.data = res.data
            // console.log(res.data);
        });

      },

   
      go_to_single(item){

        this.$router.push('fieldactivity/' + item.id);
      }

    

    },
  }
</script>