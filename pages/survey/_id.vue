<template>
<div>
     <v-toolbar flat><strong>Survey Details</strong></v-toolbar>
     <v-divider></v-divider>
     <v-simple-table>
    
    <template v-slot:default>
      <tbody>
      
         <tr>
          <th>Name</th>
          <td>{{ item.name }}</td>
        </tr>

         <tr>
          <th>Address</th>
          <td>{{ item.address }}</td>
        </tr>
         <tr>
          <th>Phone Number</th>
          <td>{{ item.pn }}</td>
        </tr>
         <tr>
          <th>Owned Rented</th>
          <td>{{ item.or }}</td>
        </tr>
         <tr>
          <th>Length of Time</th>
          <td>{{ item.lot }}</td>
        </tr>
         <tr>
          <th>Rooms</th>
          <td>{{ item.rooms }}</td>
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
          <th>Family Members</th>
          <td>
              <tr v-for="(f , i) in fms" :key="i">
                  {{f.gender}} - {{f.age}}
              </tr>
        </td>
        </tr>
         <tr>
          <th>Read Write</th>
          <td>{{ item.nprw }}</td>
        </tr>
         <tr>
          <th>Distance</th>
          <td>{{ item.distance }}</td>
        </tr>
         <tr>
          <th>Occupation</th>
          <td>{{ item.occupation }}</td>
        </tr>
         <tr>
          <th>Growth Income</th>
          <td>{{ item.gi }}</td>
        </tr>
         <tr>
          <th>Expenditures</th>
          <td>{{ item.expenditures }}</td>
        </tr>

         <tr>
          <th>Farm Size</th>
          <td>{{ item.farm_size }}</td>
        </tr>

         <tr>
          <th>Amount</th>
          <td>{{ item.amount }}</td>
        </tr>

         <tr>
          <th>Price kwh</th>
          <td>{{ item.price_kwh }}</td>
        </tr>

         <tr>
          <th>Peak Hours</th>
          <td>{{ item.peak_hours }}</td>
        </tr>
        

         <tr>
          <th>Reliability</th>
          <td>{{ item.reliability }}</td>
        </tr>

         <tr>
          <th>Fan</th>
          <td>{{ item.fan }}</td>
        </tr>

        <tr>
          <th>Fan Hours</th>
          <td>{{ item.fan_hours }}</td>
        </tr>

        <tr>
          <th>Ac</th>
          <td>{{ item.ac }}</td>
        </tr>

         <tr>
          <th>Ac Hours</th>
          <td>{{ item.ac_hours }}</td>
        </tr>

         <tr>
          <th>Computers</th>
          <td>{{ item.computers }}</td>
        </tr>

         <tr>
          <th>Computer Hours</th>
          <td>{{ item.computer_hours }}</td>
        </tr>

         <tr>
          <th>Refrigerator</th>
          <td>{{ item.refrigerator }}</td>
        </tr>

         <tr>
          <th>Refrigerator Hours</th>
          <td>{{ item.refrigerator_hours }}</td>
        </tr>

         <tr>
          <th>Savers</th>
          <td>{{ item.savers }}</td>
        </tr>

         <tr>
          <th>Savers Hours</th>
          <td>{{ item.saver_hours }}</td>
        </tr>

         <tr>
          <th>Machine</th>
          <td>{{ item.machine }}</td>
        </tr>

         <tr>
          <th>Machine Hours</th>
          <td>{{ item.machine_hours }}</td>
        </tr>

         <tr>
          <th>Tv</th>
          <td>{{ item.tv }}</td>
        </tr>

         <tr>
          <th>Tv Hours</th>
          <td>{{ item.tv_hours }}</td>
        </tr>

         <tr>
          <th>Other</th>
          <td>{{ item.other }}</td>
        </tr>

         <tr>
          <th>Other Hours</th>
          <td>{{ item.other_hours }}</td>
        </tr>

         <tr>
          <th>Other Hours</th>
          <td>{{ item.other_hours }}</td>
        </tr>

         <tr>
          <th>Feedback</th>
          <td>{{ item.feedback }}</td>
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
        resultls: "{}",
        item: {},
        user:'',
        fms:[],
        arr:[],
      }
    },
    created () {            
       
       
        this.$axios.get(`survey/${this.$route.params.id}`).then(res => {
            console.log(res.data);
            this.item = res.data;
            this.user = res.data.user.name;
            this.fms = res.data.fms;
            // this.arr = JSON.parse(res.data.survey_field);
            
        });
            
    }

  }
</script>