<template>
  <v-data-table
    :headers="headers"
    :items="data"
    :search="search"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat color="white">
        <v-toolbar-title>Survey List</v-toolbar-title>
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
          text: 'id',
          sortable: true,
          value: 'id',
        },
         {
          text: 'name',
          sortable: true,
          value: 'name',
        },
         {
          text: 'Phone Number',
          sortable: true,
          value: 'pn',
        },
         {
          text: 'Owned Rent',
          sortable: true,
          value: 'or',
        },
         {
          text: 'address',
          sortable: true,
          value: 'address',
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

        this.$axios.get('survey').then(res => {
          this.data = res.data.data
          // console.log(res.data.data);
        });

      },
      go_to_single(item){

        this.$router.push('survey/' + item.id);
      }

    

    },
  }
</script>