<template>
    <v-app dark>
      <v-navigation-drawer app v-model="drawer" :permanent="drawer" hide-overlay width="200"
    >
        <v-list>
          <v-list-tile v-for="item in items" :key="item.title">
            <v-list-tile-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-tile-action>
            <v-list-tile-content>
              <v-list-tile-title>{{ item.title }}</v-list-tile-title>
            </v-list-tile-content>
          </v-list-tile>
        </v-list>
      </v-navigation-drawer>
      <v-toolbar app>
        <v-toolbar-side-icon @click.native="drawer = !drawer"></v-toolbar-side-icon>
        <v-toolbar-title>Myfirebase</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-toolbar-items class="hidden-sm-and-down">
          <v-btn flat>Docs</v-btn>
          <v-btn flat>Github</v-btn>
          <v-btn flat>Link Three</v-btn>
          <v-btn flat>SignOut</v-btn>
        </v-toolbar-items>
      </v-toolbar>
      <v-content>
        <v-container>
          <router-view></router-view>
        </v-container>
      </v-content>
      <v-footer app></v-footer>
    </v-app>
</template>
<script>
import navbar from "./../components/partials/Navbar";
export default {
  mounted() {
    this.$auth
      .check()
      .then(user => {
        this.loggedIn = true
      })
      .catch(err => {});
  },
  data() {
    return {
      message: "Welcome Myfirebase SPA framework",
      loggedIn: false,
      items: [
        { title: 'Dashboard', icon: 'dashboard' },
        { title: 'Account', icon: 'account_box' },
        { title: 'Admin', icon: 'gavel' }
      ],
      drawer: true
    }
  },
  methods: {},
  components: {
    navbar
  }
};
</script>

<style>
.flex {
  height: 500px;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>