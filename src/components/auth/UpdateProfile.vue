<template>
  <v-card>
    <v-avatar></v-avatar>
    <v-card-media
      class="white--text"
      height="200px"
      :src="$auth.user().photoURL"
    >
    </v-card-media>
    <v-card-title>
      <div>
        <v-text-field v-model="Profile.email" label="Email"></v-text-field>
        <v-text-field v-model="Profile.displayName" label="Full name"></v-text-field>
        <v-text-field label="Avatar" prepend-icon='attach_file' @click="$refs.avatar.click()"></v-text-field>
        <input type="file" style="display: none" ref="avatar" accept="image/*" @change="getFile">
        
      </div>
    </v-card-title>
    <v-card-actions>
      <v-btn flat color="orange">Update</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>

import Profile from "./../../models/Profile";

export default {
  mounted () {
    this.$auth.check().then(user => {
      this.Profile.setPhotoURL(user.photoURL)
        .setDisplayName(user.displayName)
        .setEmail(user.email);
      this.ready = false;
    });
  },
  data () {
    return {
      message: "",
      newPhoto: null,
      error: "",
      ready: true,
      Profile: new Profile()
    };
  },
  computed: {},
  methods: {
    getFile (e) {
      this.newPhoto = e.target.files[0];
    },
    updateAvatar () {
      if (!this.newPhoto) {
        return;
      }
      this.ready = true;
      let name = this.newPhoto.name;
      this.$storage.upload({
        ref: `/images/${name}`,
        file: this.newPhoto,
        progress: snapshot => {},
        error: err => {
          this.message = err.message;
          this.$refs.snackbar.open();
        },
        completed: downloadURL => {
          this.updateProfilePicture(downloadURL);
          this.message = "Your avatar has been updated";
          this.$refs.snackbar.open();
          this.newPhoto = null;
          this.ready = false;
        }
      });
    },
    updateProfilePicture (fileName) {
      this.$auth.updateProfilePicture(fileName).then(() => {
          this.synchronize();        
      }).catch(error => {
          this.message = error.message;
          this.$refs.snackbar.open();
      })
    },
    updateProfile () {
      this.ready = true;
      this.$auth
        .user()
        .updateProfile(this.Profile.toJson())
        .then(() => {
          // Multipath Update
          this.$store.state.database
            .child("data")
            .once("value")
            .then(snapshot => {
              snapshot.forEach(childSnapshot => {
                if (
                  childSnapshot.child("email").val() == this.$auth.user().email
                ) {
                  this.$store.state.database
                    .child("data")
                    .child(childSnapshot.key)
                    .update({
                      name: this.Profile.displayName
                    });
                }
              });
            });
          this.message = "Updated";
          this.$refs.snackbar.open();
          this.ready = false;
        })
        .catch(error => {
          this.message = error.message;
          this.$refs.snackbar.open();
          this.ready = false;
        });
    },
    synchronize () {
      this.Profile.setPhotoURL(this.$auth.user().photoURL);
    }
  }
};
</script>


<style scoped>
.image {
  max-width: 200px;
  min-width: 150px;
}
.profile-card {
  margin-top: 20px;
  width: 50%;
}
.hidden {
  opacity: 0;
  width: 0;
}
</style>