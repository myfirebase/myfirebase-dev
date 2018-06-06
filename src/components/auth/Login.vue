<template>
    <div class="mdl-layout mdl-js-layout mdl-color--grey-10">
      <div class="push-down"></div>
          <main class="mdl-layout__content">
            <div class="mdl-card mdl-shadow--6dp">
              <div class="mdl-card__title mdl-color--primary mdl-color-text--white">
                <h2 class="mdl-card__title-text">Login / Register</h2>
              </div>
              <div class="mdl-card__supporting-text">
                <form action="#">
                  <div class="mdl-textfield mdl-js-textfield">
                    <input class="mdl-textfield__input" type="text" id="username" />
                    <label class="mdl-textfield__label" for="username">Username</label>
                  </div>
                  <div class="mdl-textfield mdl-js-textfield">
                    <input class="mdl-textfield__input" type="password" id="userpass" />
                    <label class="mdl-textfield__label" for="userpass">Password</label>
                  </div>
                </form>
              </div>
              <div class="mdl-card__actions mdl-card--border">
                <button class="mdl-button mdl-button--colored mdl-js-button mdl-js-ripple-effect">Log in</button>
                <button class="mdl-button mdl-button--colored mdl-js-button mdl-js-ripple-effect">Register</button>                
                <br><br>
                <button class="mdl-button google" @click="signInGoogle()">
                        <img class="icon" src="https://www.gstatic.com/firebasejs/ui/2.0.0/images/auth/google.svg"> google
                </button>
                <button class="mdl-button mdl-js-button facebook">
                        <img class="icon" src="https://www.gstatic.com/firebasejs/ui/2.0.0/images/auth/facebook.svg"> facebook
                </button>
                <button class="mdl-button twitter">
                        <img class="icon" src="https://www.gstatic.com/firebasejs/ui/2.0.0/images/auth/twitter.svg"> twitter
                </button>
                <button class="mdl-button github">
                        <img class="icon" src="https://www.gstatic.com/firebasejs/ui/2.0.0/images/auth/github.svg"> github
                </button>
              </div>
            </div>
          </main>

          <div id="demo-snackbar-example" ref="snackbar" class="mdl-js-snackbar mdl-snackbar">
            <div class="mdl-snackbar__text"></div>
            <button class="mdl-snackbar__action" type="button"></button>
        </div>

        <div class="push-down"></div>
    </div>
</template>
<script>
export default {
  mounted() {
    console.log(this.$refs.snackbar.MaterialSnackbar)
    this.$auth.logout();
    this.$auth.state("/app", "/login").then(user => {
      if (!user) {
        this.ready = true;
      }
    });
  },
  data() {
    return {
      email: "",
      password: "",
      error: "",
      ready: false
    };
  },
  computed: {},
  methods: {
    login() {
      this.ready = false;
      this.$auth
        .loginWithEmailAndPassword(this.email, this.password)
        .then(user => {
          this.ready = true;
        })
        .catch(error => {
          this.$refs.snackbar.open();
          this.ready = true;
          this.error = error.message;
        });
      this.middleware();
    },
    register() {
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
    middleware() {
      this.password = "";
    },
    signInGoogle() {
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
    signInFacebook() {
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
    signInTwitter() {
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
    signInGithub() {
      this.ready = false;
      this.$auth
        .signInWithGithub()
        .then(result => (this.ready = true))
        .catch(error => {
          this.error = error.message;
          this.ready = true;
          this.$refs.snackbar.open();
        });
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
