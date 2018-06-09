<template>
    <v-container>
      <v-form ref="form" lazy-validation>
        <v-text-field
          v-model="email"
          :rules="emailRules"
          label="E-mail"
          required
        ></v-text-field>
        <v-text-field
          v-model="password"
          :append-icon="e3 ? 'visibility' : 'visibility_off'"
          :append-icon-cb="() => (e3 = !e3)"
          :type="e3 ? 'password' : 'text'"
          name="input-10-2"
          label="Enter your password"
          hint="At least 8 characters"
          min="8"
          value=""
          required
        ></v-text-field>
        <v-btn
          @click="login"
        >
          submit
        </v-btn>
        <v-btn @click="clear">clear</v-btn>
      </v-form>
      <v-snackbar
        :timeout="6000"
        :top="true"
        :bottom="false"
        :right="false"
        :left="false"
        :multi-line="false"
        :vertical="true"
        v-model="snackbar"
      >
        {{ error }}
        <v-btn flat color="pink" @click.native="snackbar = false">Close</v-btn>
      </v-snackbar>
    </v-container>
</template>
<script>
export default {
  mounted() {
    this.$auth.logout();
    this.$auth.state("/app", "/login").then(user => {
      if (!user) {
        this.ready = true;
      }
    });
  },
  data() {
    return {
      snackbar: false,
      email: "",
      password: "",
      error: "",
      ready: false,
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'E-mail must be valid'
      ],
      e3: true
    };
  },
  computed: {},
  methods: {
    login () {
      this.ready = false;
      this.$auth.logout()
      this.$auth
        .loginWithEmailAndPassword(this.email, this.password)
        .then(user => {
          this.ready = true;
        })
        .catch(error => {
          this.snackbar = true;
          this.error = error.message;
        });
    },
    register () {
      this.ready = false;
      this.$auth
        .registerWithEmailAndPassword(this.email, this.password)
        .then(user => {
          this.ready = true;
        })
        .catch(error => {
          this.error = error.message;
          this.ready = false;
          this.$refs.snackbar.open();
        });
    },
    signInGoogle () {
      this.ready = false;
      this.$auth
        .signInWithGoogle()
        .then(result => {
          // This gives you a Google Access Token. You can use it to access the Google API.
          // console.log("Token : " + result.credential.accessToken)
          // The signed-in user info.
          // console.log("User Email : " + result.user.email)
          this.ready = true;
        })
        .catch(error => {
          this.error = error.message;
          this.ready = true;
          this.$refs.snackbar.open();
        });
    },
    signInFacebook () {
      this.ready = false;
      this.$auth
        .signInWithFacebook()
        .then(result => {
          this.ready = true;
        })
        .catch(error => {
          this.error = error.message;
          this.ready = true;
          this.$refs.snackbar.open();
        });
    },
    signInTwitter () {
      this.ready = false;
      this.$auth
        .signInWithTwitter()
        .then(result => (this.ready = true))
        .catch(error => {
          this.error = error.message;
          this.ready = true;
          this.$refs.snackbar.open();
        });
    },
    signInGithub () {
      this.ready = false;
      this.$auth
        .signInWithGithub()
        .then(result => (this.ready = true))
        .catch(error => {
          this.error = error.message;
          this.ready = true;
          this.$refs.snackbar.open();
        });
    },
    clear () {
    this.$refs.form.reset()
  }
  }
};
</script>


<style scoped>
  .push-down {
    margin-top: 15px;
  }

  .icon {
    height: 18px;
    width: 18px;
    margin-right: 8px;
  }
  .mdl-layout {
    align-items: center;
    justify-content: center;
  }
  .mdl-layout__content {
    padding: 24px;
    flex: none;
  }

  .facebook {
    background: #3b5998;
    color: white;
    width: 100%;
  }

  .github {
    background: black;
    color: #fff;
    width: 100%;
  }

  .twitter {
    background: #1da1f2;
    color: #fff;
    width: 100%;
  }

  .google {
    background: #db4437;
    color: #fff;
    width: 100%;
  }
</style>
