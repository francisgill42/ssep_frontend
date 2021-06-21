<template>
<v-app>
<v-navigation-drawer
v-model="drawer" :clipped="clipped" fixed app>
<v-list>
<v-list-item>
<v-list-item-content>
<img src="/ssep-logo.png" alt="ssep login" style="width:50px; height:auto">
</v-list-item-content>
</v-list-item>

<v-list-item v-for="(item, i) in menus" :key="i" :to="item.to" router exact>
<v-list-item-action>
<v-icon style="border-radius:50%; padding:7px;" class="secondary white--text">{{ item.icon }}</v-icon>
</v-list-item-action>
<v-list-item-content>
<v-list-item-title  v-text="item.title"/>
</v-list-item-content>
</v-list-item>
</v-list>
</v-navigation-drawer>

<v-app-bar app class="secondary">
<v-app-bar-nav-icon @click.stop="drawer = !drawer" class="white--text"/>
<!-- {{title}} -->
<span class="white--text" v-if="this.$auth.user" > Welcome, <b>{{this.$auth.user.email}}</b></span>
  <!-- &nbsp;<b>{{this.$auth.user.role.role}}</b> -->
  <v-spacer />
<v-icon class="white--text" @click="logout">{{ logout_btn.icon }}</v-icon>
<!-- <span v-if="this.$auth.user" >  -->

<!-- <v-btn text> -->
<!-- {{logout_btn.label}} -->

<!-- </v-btn> -->
<!-- </span> -->
</v-app-bar>
<v-main>

<v-container>
<nuxt />
</v-container>

</v-main>

<v-footer :fixed="fixed" app class="primary white--text">
<span>&copy; {{year}}</span>
</v-footer>
</v-app>
</template>

<script>
export default {

data () {
return {
year: new Date().getFullYear(),
clipped: false,
fixed:false,
drawer: true,
menus : [],

miniVariant: false,
right: true,
rightDrawer: false,
title: 'Virtual School',
logout_btn:{
icon:'mdi-logout',
label:'Logout'
}
}
},
mounted () {
  this.get_menus();
},
methods:{
async logout() {
  await this.$auth.logout();
},
get_menus () {


  var user_and_subuser_menu = [
            { icon: 'mdi-apps', title: 'Home',to: '/' },
            { icon: 'mdi-chart-bubble', title: 'Job',to: '/job' },
  ];


var user_callagent = [
            // { icon: 'mdi-apps', title: 'Home',to: '/' },
           { icon: 'mdi-file-document', title: 'Complains',to: '/complain' },
  ];


  var admin_and_subadmin_menu = [
            { icon: 'mdi-apps', title: 'Home',to: '/' },
            { icon: 'mdi-chart-bubble', title: 'Job',to: '/job' },
            { icon: 'mdi-account', title: 'Users',to: '/user' },
            // { icon: 'mdi-surround-sound', title: 'Survey',to: '/survey' },
            { icon: 'mdi-file-document', title: 'Complains',to: '/complain' },

             { icon: 'mdi-file-document', title: 'Beneficiary Form',to: '/beneficiaryform' },
            { icon: 'mdi-file-document', title: 'Field Activity Form',to: '/fieldactivity' },
            { icon: 'mdi-file-document', title: 'Call Center Form',to: '/callcenterform' },

            { icon: 'mdi-codepen', title: 'Department',to: '/department' },
            { icon: 'mdi-codepen', title: 'Department',to: '/department' },
            { icon: 'mdi-image-area', title: 'District',to: '/district' },
            { icon: 'mdi-chart-areaspline', title: 'Reports',to: '/reports' },
  ];



  var monitoring_menu = [
            { icon: 'mdi-apps', title: 'Home',to: '/' },
            { icon: 'mdi-chart-bubble', title: 'Job',to: '/job' },
            // { icon: 'mdi-surround-sound', title: 'Survey',to: '/survey' },
             { icon: 'mdi-file-document', title: 'Complains',to: '/complain' },

            { icon: 'mdi-file-document', title: 'Beneficiary Form',to: '/beneficiaryform' },
            { icon: 'mdi-file-document', title: 'Field Activity Form',to: '/fieldactivity' },
            { icon: 'mdi-file-document', title: 'Call Center Form',to: '/callcenterform' },
            { icon: 'mdi-codepen', title: 'Department',to: '/department' },

            { icon: 'mdi-chart-areaspline', title: 'Reports',to: '/reports' },
  ];

  var all_menu = [
            { icon: 'mdi-apps', title: 'Home',to: '/' },
            { icon: 'mdi-chart-bubble', title: 'Job',to: '/job' },
            { icon: 'mdi-account', title: 'Users',to: '/user' },
            // { icon: 'mdi-surround-sound', title: 'Survey',to: '/survey' },

            { icon: 'mdi-file-document', title: 'Complains',to: '/complain' },

            { icon: 'mdi-file-document', title: 'Beneficiary Form',to: '/beneficiaryform' },
            { icon: 'mdi-file-document', title: 'Field Activity Form',to: '/fieldactivity' },
            { icon: 'mdi-file-document', title: 'Call Center Form',to: '/callcenterform' },
            { icon: 'mdi-codepen', title: 'Department',to: '/department' },
            { icon: 'mdi-image-area', title: 'District',to: '/district' },
            { icon: 'mdi-briefcase-check', title: 'Role',to: '/role' },
            { icon: 'mdi-arrange-bring-forward', title: 'Status',to: '/status' },
            { icon: 'mdi-chart-areaspline', title: 'Reports',to: '/reports' },
  ];


if(this.$auth.user){
  if(this.$auth.user.master){
    this.menus = all_menu;
  }
  else {
      if(this.$auth.user.role_id == 4){
        this.menus = monitoring_menu;
      }
      else if(this.$auth.user.role_id == 1 || this.$auth.user.role_id == 2){
        this.menus = admin_and_subadmin_menu;
      }
      else if(this.$auth.user.role_id == 3){
        this.menus = user_and_subuser_menu;
      }

       else if(this.$auth.user.role_id == 8){
        this.menus = user_callagent;
      }
      else{
        this.menus = [];
      }
  }
}

}
}
}
</script>

<style>
.grad {
  background: linear-gradient(to bottom right, #0e97e7, grey);
}
</style>
