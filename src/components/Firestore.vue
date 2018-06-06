<template>
    <div class = "container" id="app">
        <ul class="list-group">
            <li class="list-group-item" v-for= "p in Persons" v-bind:key="p['.key']">
            {{p.name}} <button @click="remove(p)" class="badge">x</button>
            </li>
        </ul>
        
        <input class="form-control" type="text" v-model="Person.name" v-on:keyup.enter="add()">
        <br>
        <button class="btn btn-primary" @click="add()">Add new</button>
        <br><br><br>
        <p>{{zamantoti.name}}</p>
    </div>
</template>

<script>

import Person from "./../models/Person"

export default {
    created() {

    },
    mounted() {
        // this.$auth.check({
        //     then: (user) => {
                
        //     },
        //     catch: () => {
        //         // user is not logged in
        //     }
        // })
        //console.log(this.Houssain)
        // this.$destroy()

    console.log(this.Persons)
    },
    firestore() {
        return {
            Persons: this.$store.state.firestore.collection("Persons"),
            zamantoti: this.$store.state.firestore.doc("Persons/zamantoti")
            
        }
    },
    data() {
        return {
            username: "",
            Person: new Person(this.$store.state.firestore.collection('Persons')).init()
        }
    },
    methods: {
        add() {
            console.log(this.Person)
            this.Person.add().then((success) => {
                this.Person.name = ""
                console.log("ASDA")
            }).catch(error => {
                console.log(error.message)
            })
        },
        remove(e) {
            this.Person.delete(e['.key']).then(() => {
                console.log("Done")
            }).catch(error => {
                console.log(error.message)
            })
        }
    }
}
</script>

<style scoped>
.container {
    margin: 0 auto;
}

.data-container {
    width: 700px;
}
</style>