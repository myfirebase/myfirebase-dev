<template>
  <v-layout>
    <v-list v-for="(item, index) in data" :key="index">
      <v-list-tile-content>
        {{item.name}}
      </v-list-tile-content>
    </v-list>
  </v-layout>
</template>

<script>

import Item from "./../models/Item";

export default {
  mounted() {
    this.$auth.check()
    .then(user => {
        // this.$route.push('/login')
        // this.$route.go(1)
        console.log(this.data)
    })
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
  firebase() {
    return {
      data: this.$store.state.database.child("item")
    };
  },
  data() {
    return {
      token: "",
      Item: new Item(this.$store.state.database.child('item')).init()
    }
  },
  methods: {
    addItem() {
      this.$firebaseRefs.data.onDisconnect().cancel()
      this.Item.push()
      this.Item.name = ""
    },
    deleteItem(key) {
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