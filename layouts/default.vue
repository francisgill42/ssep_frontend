<template>
<v-app>
<v-navigation-drawer 
v-model="drawer" :clipped="clipped" fixed app>
<v-list>
<v-list-item>
<v-list-item-content>
<img src="energy-logo.png" alt="ssep login">
</v-list-item-content>
</v-list-item>

<v-list-item v-for="(item, i) in menus" :key="i" :to="item.to" router exact>
<v-list-item-action>
<v-icon style="border-radius:50%; padding:7px;" class="grey white--text">{{ item.icon }}</v-icon>
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
<span class="white--text" v-if="this.$auth.user" > Welcome, <b>{{this.$auth.user.name}}</b></span>
<v-spacer />
<v-icon class="white--text" @click="logout">{{ logout_btn.icon }}</v-icon>
<!-- <span v-if="this.$auth.user" > Welcome, <b>{{this.$auth.user.name}}</b> -->
<!-- <v-btn text> -->
<!-- {{logout_btn.label}} -->

<!-- </v-btn> -->
<!-- </span> -->
</v-app-bar>
<v-content>

<v-container>
<nuxt />
</v-container>

</v-content>

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
menus : [
  { icon: 'mdi-apps', title: 'Home',to: '/' },
  { icon: 'mdi-chart-bubble', title: 'Job',to: '/job' },
  { icon: 'mdi-account', title: 'Users',to: '/user' },
  { icon: 'mdi-surround-sound', title: 'Survey',to: '/survey' },
  { icon: 'mdi-codepen', title: 'Department',to: '/department' },
  { icon: 'mdi-image-area', title: 'District',to: '/district' },
  { icon: 'mdi-briefcase-check', title: 'Role',to: '/role' },

],      

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

methods:{
async logout() {
  await this.$auth.logout();
},
}
}
</script>

<style>
.grad {
  background-image: linear-gradient(to bottom right, #008b09, grey);
}
/* .grad-opp {
  background-image: linear-gradient(to bottom right, white, white);
} */
</style>
