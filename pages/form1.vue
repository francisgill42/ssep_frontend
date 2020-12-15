<template>
<v-card flat outlined>
  <v-card-title>
    <v-row class="text-center">
      <v-col cols="12" sm="12" md="12">
        <h2>Economic and Demographic Background</h2>
      </v-col>
    </v-row>
    </v-card-title>
  <v-card-text>
    <v-container>
      <form>
        <v-row>
          <v-col cols="12" sm="12" md="12">
            <p class="headline">Household:  </p>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="4" sm="4" md="4">
            <v-text-field
              v-model="name"
              :error-messages="nameErrors"
              label="Name Head of Household"
              required
              
            ></v-text-field>
          </v-col>
          <v-col cols="4" sm="4" md="4">
            <v-text-field
              v-model="address"
              :error-messages="nameErrors"
              label="Address"
              required
              
            ></v-text-field>
          </v-col>
          <v-col cols="4" sm="4" md="4">
            <v-text-field
              v-model="pn"
              :error-messages="nameErrors"
              label="Phone Number"
              required
              
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="4" sm="4" md="4">
            <p>Dwelling owned or rented</p>
            <v-radio-group
              v-model="radios"
              row
            >
           
              <v-radio
                label="Owned"
                value="radio-1"
              ></v-radio>
              <v-radio
                label="Rent"
                value="radio-2"
              ></v-radio>
            </v-radio-group>
          </v-col>
          <v-col cols="4" sm="4" md="4">
            <p>Length of time in dwelling</p>
            <v-radio-group
              v-model="radios"
              row
            >
              <v-radio
                label="5"
                value="radio-1"
              ></v-radio>
              <v-radio
                label="10"
                value="radio-2"
              ></v-radio>
              <v-radio
                label="20"
                value="radio-3"
              ></v-radio>
            </v-radio-group>
          </v-col>
          <v-col cols="4" sm="4" md="4">
            <v-text-field
              v-model="rooms"
              :error-messages="nameErrors"
              label="Number of Rooms"
              required
              
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12" sm="12" md="12">
            <p class="headline">Composition of household </p>
            <p class="black--text">Not including yourself, what are the gender and age of the people living in your home more than 50% of time ?</p>
            <v-radio-group
              v-model="radios"
              row
            >
              <v-radio
                label="Male"
                value="radio-1"
              ></v-radio>
              <v-radio
                label="Female"
                value="radio-2"
              ></v-radio>
            </v-radio-group>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="6" sm="6" md="6">
            <v-text-field
              label="Number of people who can read and write ?"
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="6" sm="6" md="6">
            <v-text-field
              label="Distance of household from transport network ?"
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="6" sm="6" md="6">
            <v-text-field
              label="Occupation of head of household"
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="6" sm="6" md="6">
            <v-text-field
              label="Cross Household Income"
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="6" sm="6" md="6">
            <v-text-field
              label="Family expenditures"
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="6" sm="6" md="6">
            <p class="headline">For farm households </p>
            <v-text-field
              label="Size of farm"
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12" sm="12" md="12" class="text-center">
            <v-btn
              color="secondary"
              @click="submit"
            >
              Next
            </v-btn>
          </v-col>
        </v-row>
      </form>
    </v-container>
  </v-card-text>
  </v-card>
</template>

<script>

  export default {
    data () {
      return {
        
       
      }
    },
   

  
   
  }
</script>

<style>

</style>