<template>
    <div>
      <v-app>
        <v-navigation-drawer app></v-navigation-drawer>
        <v-toolbar app></v-toolbar>
        <v-content>
          <v-container>Hello world</v-container>
        </v-content>
        <v-footer app></v-footer>
      </v-app>
    </div>
</template>
<script>
import navbar from "./../components/partials/Navbar";
export default {
  mounted() {
    this.$auth
      .check()
      .then(user => {})
      .catch(err => {});
  },
  data() {
    return {
      message: "Welcome Myfirebase SPA framework",
      userLogged: false
    };
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