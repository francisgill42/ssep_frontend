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
          <th>Activity Name</th>
          <td>{{ item.activity_name }}</td>
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
          <th>Distric Officer Name</th>
          <td>{{ item.village }}</td>
        </tr>
        
        <tr>
          <th>No of Participants  </th>
          <td>{{ item.count_participants }}</td>
        </tr>

        <tr>
          <th>No of Male </th>
          <td>{{ item.male }}</td>
        </tr>


        <tr>
          <th>No of Female </th>
          <td>{{ item.female }}</td>
        </tr>

         <tr>
          <th>Details </th>
          <td>{{ item.details }}</td>
        </tr>

 


          <tr>
          <th>Attachments</th>

          <td>
            <v-row>
                <v-col
                cols="2"
                v-for="(img,i) in item.images" :key="i">
                 

                    <v-dialog v-model="dialog1" max-width="900px">
                      
                    <template v-slot:activator="{ on }">
                    <div class="pa-5">
                    <v-img v-on="on" height="175px" width="250px" :src="img"></v-img>
                    </div>
                    </template>
                    <v-img height="auto" width="100%" :src="img"></v-img>

                    </v-dialog> 

                </v-col>

            </v-row>
          </td>
          </tr>

          <tr>
          <th>Attendance Sheets</th>

          <td>
            <v-row>
                <v-col
                cols="2"
                v-for="(img,i) in item.attendance_sheet" :key="i">

                    <v-dialog v-model="dialog1" max-width="900px">
                    
                    <template v-slot:activator="{ on }">
                    <div class="pa-5">
                    <v-img v-on="on" height="175px" width="250px" :src="img"></v-img>
                    </div>
                    </template>
                    <v-img height="auto" width="100%" :src="img"></v-img>

                    </v-dialog> 

                </v-col>

            </v-row>
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
       
       
        this.$axios.get(`fieldactivity/${this.$route.params.id}`).then(res => {
            console.log(res.data);
            this.item = res.data;
            this.user = res.data.user.name;
            this.fms = res.data.fms;
            // this.arr = JSON.parse(res.data.survey_field);
            
        });
            
    }

  }
</script>