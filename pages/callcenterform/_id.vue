<template>
<div>
     <v-toolbar flat><strong>Activity Details</strong></v-toolbar>
     <v-divider></v-divider>
     <v-simple-table>
    
    <template v-slot:default>
      <tbody>
      
         <tr>
          <th>Form No</th>
          <td>{{ item.form_no }}</td>
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
          <th>Nearest Landmark</th>
          <td>{{ item.n_landmark }}</td>
        </tr>

         <tr>
          <th>Address</th>
          <td>{{ item.address }}</td>
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
          <th>Male</th>
          <td>{{ item.male_count }}</td>
        </tr>

         <tr>
          <th>Female</th>
          <td>{{ item.female_count }}</td>
        </tr>

        <tr>
          <th>Children</th>
          <td>{{ item.children_count }}</td>
        </tr>

        <tr>
          <th>Source of information</th>
          <td>{{ item.source_of_info }}</td>
        </tr>

        <tr>
          <th>Electricity</th>
          <td>{{ item.electricity }}</td>
        </tr>

        <tr>
          <th>Load shedding hours</th>
          <td>{{ item.loadshedding_hours }}</td>
        </tr>

        <tr>
          <th>Micro Finance Load</th>
          <td>{{ item.mocrofinanceload }}</td>
        </tr>

        <tr>
          <th>MFI</th>
          <td>{{ item.mfi }}</td>
        </tr>

         <tr>
          <th>Date</th>
          <td>{{ item.date }}</td>
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


       
         

<!--         
         <tr>
          <th>Family Members</th>
          <td>
              <tr v-for="(f , i) in fms" :key="i">
                  {{f.gender}} - {{f.age}}
              </tr>
        </td> -->


        
        
        <tr>
          <th>User</th>
          <td>{{ user }}</td>
        </tr>

         <tr>
          <th>Submitted</th>
          <td>{{ item.created_at }}</td>
        </tr>

        <tr>
          <th>Date</th>
          <td>{{ item.date }}</td>
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
        resultls: "{}",
        item: {},
        user:'',
        fms:[],
        arr:[],
      }
    },
    created () {            
       
       
        this.$axios.get(`callcenterform/${this.$route.params.id}`).then(res => {
            console.log(res.data);
            this.item = res.data;
            this.user = res.data.user.name;
            this.fms = res.data.fms;
            // this.arr = JSON.parse(res.data.survey_field);
            
        });
            
    }

  }
</script>