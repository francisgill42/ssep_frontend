<template>
<div>
     <v-toolbar flat><strong>Activity Details</strong></v-toolbar>
     <v-divider></v-divider>
     <v-simple-table>
    
    <template v-slot:default>
      <tbody>
      
      <tr>
          <th>#</th>
          <td>{{ item.id }}</td>
        </tr>

         <tr>
          <th>Name</th>
          <td>{{ item.name }}</td>
        </tr>

        <tr>
          <th>Father's Name</th>
          <td>{{ item.father_name }}</td>
        </tr>

        <tr>
          <th>Cnic</th>
          <td>{{ item.cnic }}</td>
        </tr>

        <tr>
          <th>District</th>
          <td>{{ item.district }}</td>
        </tr>

        <tr>
          <th>UC</th>
          <td>{{ item.uc }}</td>
        </tr>

        <tr>
          <th>Taluka</th>
          <td>{{ item.taluka }}</td>
        </tr>

         <tr>
          <th>Village</th>
          <td>{{ item.village }}</td>
        </tr>

         <tr>
          <th>Event</th>
          <td>{{ item.activity_event }}</td>
        </tr>

         <tr>
          <th>Supplier Name</th>
          <td>{{ item.supplier_name }}</td>
        </tr>

         <tr>
          <th>Attachment</th>
          <td>

            <v-dialog v-model="dialog1" max-width="900px">
            <template v-slot:activator="{ on }">
            <div class="pa-5">
                  <v-img v-on="on" height="175px" width="250px" :src="item.attachment"></v-img>
            </div>
            </template>

            <v-img height="auto" width="100%" :src="item.attachment"></v-img>
            </v-dialog> 

          </td>
        </tr>
         
           <tr>
          <th>Map latitude</th>
          <td>{{ item.map_lat }}</td>
        </tr> 

        <tr>
          <th>Map longitude</th>
          <td>{{ item.map_long }}</td>
        </tr> 

        <tr>
          <th>Location </th>
          <td>{{ item.map_location }}</td>
        </tr> 


         <tr>
          <th>User</th>
          <td>{{ user }}</td>
        </tr> 

         <tr>
          <th>Submitted</th>
          <td>{{ item.created_at }}</td>
        </tr>



      </tbody>
    </template>
  </v-simple-table>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        dialog1 : '',
        resultls: "{}",
        item: {},
        user:'',
        fms:[],
        arr:[],
      }
    },
 
    mounted () {            
              
        this.$axios.get(`beneficiaryform/${this.$route.params.id}`).then(res => {
            console.log(res.data);
            this.item = res.data;
            
            this.user = res.data.user.name || '';
            this.fms = res.data.fms;
            // this.arr = JSON.parse(res.data.survey_field);
            
        });
            
    }

  }
</script>