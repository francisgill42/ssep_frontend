<template>

<div>
    <v-btn
    :color="btn_class"
    dark
    :small="size"
    @click="dialog = true"
    >
    {{ revision_title }}
    </v-btn>

    <v-dialog v-model="dialog"  persistent max-width="600px">
   
    <v-card>

    <v-container>

 
    <v-form
    ref="form"
    lazy-validation
    >
    <v-toolbar flat class="primary mb-3" dark><strong>Revision</strong>
    <v-spacer></v-spacer>
    <v-btn small class="secondary lighten-2" @click="onPick_attachment">
    {{get_attachment_name}}
    <v-icon> mdi-upload</v-icon>
    </v-btn>   
    <input required multiple type="file" @change="check_attachment" style="display:none;" accept="image/*" ref="attachmentInput">
    </v-toolbar>

    <v-row>
    <v-col cols="12">
    <v-textarea
    rows="3"
    class="px-5"
    v-model="msg"
    :rules="Rules"
    label="Item"
    required
    ></v-textarea>

    </v-col>
    <v-col cols="2">
    <v-btn
    color="secondary lighten-2"
    class="mr-4 mx-5"
    @click="add_revision"
    small
    >
    Save
    </v-btn>
    </v-col>
    <v-col cols="2">
    <v-btn
    color="primary"
    class="mr-4"
    @click="dialog = false"
    small
    >
    close
    </v-btn>
    </v-col>
    </v-row>
    </v-form>

    </v-container>
    </v-card>
    </v-dialog>
   
    </div>
</template>


<script>

export default {

props : ['job_id','item','btn_class','size','revision_title'],    
data: () => ({
dialog:false,    
me:{},
created_by:'',
assigned_to:'',
department:'',
district:'',
keyword:'',
job_type:'',

user_id :'',
msg : '',
attachment : '',
r_id:'',
Rules : [
v => !!v || 'This field is required',
],
}),
 created () {   
  
  this.me = this.$auth.user;

},
watch: { },
methods : { 
        onPick_attachment () { 
        this.$refs.attachmentInput.click() 
        },

        check_attachment(e) { 
        this.attachment = e.target.files || ''; 
        },

        add_revision () {


        if(this.$refs.form.validate()){
         

          var revision = {
          job_id : this.job_id,
          s_id : this.$auth.user.id,
          r_id : this.me.master || this.me.role_id == 3 || this.$auth.user.role_id == 4 || this.$auth.user.role_id == 3 ? this.item.created_by : this.item.assigned_to ,
          msg : this.msg
          }
            this.$axios.post(`revision`,revision).then(res => {
            
            this.$nuxt.$emit('revision',res.data.data);

              let payload = new FormData();

              for(var j = 0; j < this.attachment.length; j++){
                payload.append(`attachment[${j}]`, this.attachment[j]);  
              } 

              this.$axios.post('update_attachment/' + this.job_id, payload)
                .then(res => {
                  
                  this.item.attachment = res.data.attachment

                  this.$nuxt.$emit('update_attachment',res.data.attachment);

                  this.dialog = false
                  this.$refs.form.reset()
                
              }).catch(error => console.log(error));
            
        });
         }

        }
},

  computed : {
         get_attachment_name(){
             return this.attachment.name ? this.attachment.name : 'Upload Attachment';
        },
    },

}

</script>