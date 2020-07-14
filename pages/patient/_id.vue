<template>
  <div>
    <v-row>
      <v-col cols="7">
        <v-expansion-panels 
      v-model="panel"
      :readonly="readonly"
      multiple
    >
      <v-expansion-panel>
        <v-expansion-panel-header>Diagnosis</v-expansion-panel-header>
        <v-expansion-panel-content>
           <v-simple-table
      :dense="dense"
      :fixed-header="fixedHeader"
      :height="height"
    >
      <template v-slot:default>
        <tbody>
          <tr>
            <td>Contact of B+ PT B Case</td>
            <td>{{ data.diagnosis.contact_of_ptb_case }}</td>
          </tr>
          <tr>
            <td>Diabetes</td>
            <td>{{ data.diagnosis.diabetes }}</td>
          </tr>
          <tr>
            <td>Malnutrition</td>
            <td>{{ data.diagnosis.malnutrition }}</td>
          </tr>
          <tr>
            <td>HIV Infection / AIDS</td>
            <td>{{ data.diagnosis.hiv_inf_aids }}</td>
          </tr>
          <tr>
            <td>Smoking</td>
            <td>{{ data.diagnosis.smoking }}</td>
          </tr>
          <tr>
            <td>Health care worker (HCW)</td>
            <td>{{ data.diagnosis.hcw }}</td>
          </tr>
          <tr>
            <td>Other (specify also if treated for LTBI)</td>
            <td>{{ data.diagnosis.risk_f_other }}</td>
          </tr>
          <tr>
            <td>Pulmonary</td>
            <td>{{ data.diagnosis.pulmonary }}</td>
          </tr>
          <tr>
            <td>Extra Pulmonary</td>
            <td>{{ data.diagnosis.e_pulmonary }}</td>
          </tr>
          <tr>
            <td>pleura</td>
            <td>{{ data.diagnosis.pleura }}</td>
          </tr>
          <tr>
            <td>lymph_node</td>
            <td>{{ data.diagnosis.lymph_node }}</td>
          </tr>
          
          <tr>
            <td>abdomen</td>
            <td>{{ data.diagnosis.abdomen }}</td>
          </tr>
          <tr>
            <td>bones</td>
            <td>{{ data.diagnosis.bones }}</td>
          </tr>
          <tr>
            <td>Other disease site</td>
            <td>{{ data.diagnosis.d_site_other }}</td>
          </tr>
          <tr>
            <td>histology</td>
            <td>{{ data.diagnosis.histology }}</td>
          </tr>
          <tr>
            <td>x_ray</td>
            <td>{{ data.diagnosis.x_ray }}</td>
          </tr>
          <tr>
            <td>U/sound histology</td>
            <td>{{ data.diagnosis.u_sound }}</td>
          </tr>
          <tr>
            <td>MRI</td>
            <td>{{ data.diagnosis.mri }}</td>
          </tr>
          <tr>
            <td>Other (EPTB)</td>
            <td>{{ data.diagnosis.eptb_other }}</td>
          </tr>

          <tr>
            <td>Patient Type</td>
            <td>{{ data.diagnosis.tb_patient_type }}</td>
          </tr>
          <tr>
            <td>Bacteriology Confirmed B+</td>
            <td>{{ data.diagnosis.bacteriology_confirmed }}</td>
          </tr>
          <tr>
            <td>Clinically Diagnosed</td>
            <td>{{ data.diagnosis.clinically_diagnosed }}</td>
          </tr>
          <tr>
            <td>Retreatment</td>
            <td>{{ data.diagnosis.retreatment }}</td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
        </v-expansion-panel-content>
      </v-expansion-panel>


      <v-expansion-panel v-for="(f,i) in data.followup" :key="i">
        <v-expansion-panel-header>{{ f.visits }} followup</v-expansion-panel-header>
        <v-expansion-panel-content>
          <h3 style="margin: 15px;">Investigation</h3>
          <v-simple-table
      :dense="dense"
      :fixed-header="fixedHeader"
      :height="height"
    >
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-left">Date</th> 
            <th class="text-left">Examination Type</th> 
            <th class="text-left">Lab No</th> 
            <th class="text-left">Lab Name</th> 
            <th class="text-left">Result</th> 
            <th class="text-left">CXR</th> 
            <th class="text-left">Weight</th> 
          </tr>
        </thead>
        <tbody>
          <tr v-for="(inv,i) in f.investigation" :key="i">
            <td>{{ inv.date }}</td>
            <td>{{ inv.examination_type }}</td>
            <td>{{ inv.lab_no }}</td>
            <td>{{ inv.lab_name }}</td>
            <td>{{ inv.result }}</td>
            <td>{{ inv.cxr }}</td>
            <td>{{ inv.weight }}</td>
          </tr>
          
        </tbody>
      </template>
    </v-simple-table>

    <h3  style="margin: 15px;">Treatment</h3>
    <v-simple-table
      :dense="dense"
      :fixed-header="fixedHeader"
      :height="height"
    >
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-left">Regimen Type</th> 
            <th class="text-left">Drug Regimen</th> 
            <th class="text-left">Dosage</th> 
            <th class="text-left">Tablets</th> 
            <th class="text-left">Taken with</th> 
          </tr>
        </thead>
        <tbody>
          <tr v-for="(treat,i) in f.treatment" :key="i">
            <td>{{ treat.regimen_type }}</td>
            <td>{{ treat.drug_regimen }}</td>
            <td>{{ treat.dosage }}</td>
            <td>{{ treat.tablets }}</td>
            <td>{{ treat.taken_with }}</td>
          </tr>
          
        </tbody>
      </template>
    </v-simple-table>

        </v-expansion-panel-content>
      </v-expansion-panel>


    </v-expansion-panels>
      </v-col>
      <v-col cols="5">
        <v-card>
          <v-simple-table
      :dense="dense"
      :fixed-header="fixedHeader"
      :height="height"
    >
      <template v-slot:default>
        <thead class="teal">
          <tr>
            <th class="text-left white--text" colspan="2"><h3>Basic Details</h3></th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Name</td>
            <td>{{ data.p_name }}</td>
          </tr>
          <tr>
            <td>CNIC</td>
            <td>{{ data.p_cnic }}</td>
          </tr>
          <tr>
            <td>Mobile</td>
            <td>{{ data.p_mobile_no }}</td>
          </tr>
          <tr>
            <td>Address</td>
            <td>{{ data.p_address }}</td>
          </tr>
          <tr>
            <td>Occupation</td>
            <td>{{ data.p_occupation }}</td>
          </tr>
          <tr>
            <td>Gender</td>
            <td>{{ data.p_gender }}</td>
          </tr>
          <tr>
            <td>Reffered by</td>
            <td>{{ data.reffered_by }}</td>
          </tr>
          <tr>
            <td>Date of Birth</td>
            <td>{{ data.dob }}</td>
          </tr>
          <tr>
            <td>Case No</td>
            <td>{{ data.case_no }}</td>
          </tr>
          
          <tr>
            <td>Age</td>
            <td>{{ data.age }}</td>
          </tr>
          <tr>
            <td>Status</td>
            <td>{{ data.status }}</td>
          </tr>
          <tr>
            <td>Supporter Name</td>
            <td>{{ data.supporter_name }}</td>
          </tr>
          <tr>
            <td>Supporter Contact</td>
            <td>{{ data.supporter_contact }}</td>
          </tr>
          <tr>
            <td>Supporter Type</td>
            <td>{{ data.supporter_type }}</td>
          </tr>
          <tr>
            <td>Doctor</td>
            <td>{{ data.doctor }}</td>
          </tr>
          <tr>
            <td>Field Officer</td>
            <td>{{ data.fo }}</td>
          </tr>
          <tr>
            <td>Registration Date</td>
            <td>{{ data.reg_date }}</td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
  export default {
    data: () => ({
     
     
      data: {},
      
     
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      },
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
    },

    created () {
      this.initialize()
    },

    methods: {
      initialize () {
      console.log(this.$route.params.id)
      this.$axios.get('patient_details/'+ this.$route.params.id).then(res => {

          this.data = res.data
          console.log(res.data);
        });
      },      
    },
  }
</script>