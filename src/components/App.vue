<template>
<v-layout row>
    <v-flex xs12 sm6 offset-sm3>
      <v-card row>
        <v-list two-line subheader>
          <v-subheader>General</v-subheader>
          <v-list-tile v-for="(item, index) in data" :key="index" avatar>
            <v-list-tile-content>
              <v-list-tile-title>{{item.name}}</v-list-tile-title>
            </v-list-tile-content>
          </v-list-tile>
        </v-list>
        <v-container>
        <v-text-field
        label="Item Name"
        v-model="Item.name"
        ></v-text-field>
        <v-btn @click="addItem()">Success</v-btn>
        </v-container>
      </v-card>
    </v-flex>
</v-layout>
</template>

<script>

import Item from "./../models/Item";

export default {
  mounted () {
    this.$auth.check()
    .then(user => { })
    .catch(err => {
      console.log(err)
    })

    // retrieve messaging token.
    // only for production, you have to register the serviceworker.

    // this.$store.state.messaging.getToken()
    //     .then((token) => {
    //         this.token = token
    //     })
  },
  firebase () {
    return {
      data: this.$store.state.database.child("item")
    };
  },
  data () {
    return {
      token: "",
      Item: new Item(this.$store.state.database.child('item')).init()
    }
  },
  methods: {
    addItem () {
      this.$firebaseRefs.data.onDisconnect().cancel()
      this.Item.push()
      this.Item.name = ""
    },
    deleteItem (key) {
      this.Item.remove(key);
    }
  }
};
</script>

<style scoped>
.delete-data {
  cursor: pointer;
  color: red;
}

.data-container {
  width: 700px;
}

</style>