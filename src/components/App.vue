<template>
    <div class = "container" id="app">
        <ul class="list-group">
            <li class="list-group-item" v-for= "p in data" v-bind:key="p['.key']">
            {{p.name}} <button @click="remove(p)" class="badge">x</button>
            </li>
        </ul>
        
        <input class="form-control" type="text" v-model="Item.name" v-on:keyup.enter="addItem()">
        <br>
        <button class="btn btn-primary" @click="addItem()">Add new</button>
    </div>
</template>

<script>

import Item from "./../models/Item";

export default {
  mounted() {
    this.$auth.check()
    .then(user => {
        // this.$route.push('/login')
        // this.$route.go(1)
    })
    .catch(err => {[console.log(err)]})

    // retrieve messaging token.
    // only for production, you have to register the serviceworker.
    /*
        this.$store.state.messaging.getToken()
            .then((token) => {
                this.token = token
            })
        */
  },
  firebase() {
    return {
      data: this.$store.state.database.child("persons")
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
      this.$firebaseRefs.data.onDisconnect().cancel();
      this.Item.push();
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