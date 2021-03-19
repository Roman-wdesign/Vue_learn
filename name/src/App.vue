<template>
  <div class="container">
    <form class="card" @submit.prevent="createPerson">
      <h2>Work with database</h2>

      <div class="form-control">
        <label for="name">Enter your name</label>
        <input type="text" id="name" v-model.trim="name">
      </div>
      <button class="btn primary" :disabled="name.length === 0">To create a man</button>
    </form>

    <app-people-list
        :people="people"
        @load="loadPeople"
    ></app-people-list>

  </div>
</template>

<script>
import axios from 'axios';
import AppPeopleList from "./components/AppPeopleList";

export default {
data () {
  return {
    name:'',
    people:[]
  }
 },
  methods:{
    async createPerson(){
      const response = await fetch('https://vue-with-http-9798d-default-rtdb.europe-west1.firebasedatabase.app/people.json',{
      method: 'POST',
        headers: {
        'Content-type': 'application/json'
        },
        body: JSON.stringify({
          firstName: this.name
        })
      })
      const firebaseData = await response.json()
      console.log(firebaseData)
      this.name=''
    },
    async loadPeople() {
     const {data} = await axios.get('https://vue-with-http-9798d-default-rtdb.europe-west1.firebasedatabase.app/people.json')
      this.people = Object.keys(data).map( key=> {
          return{
            id: key,
            ... data[key]
          }
      })

    }
  },
  components: {AppPeopleList}
}
</script>

<style>

</style>