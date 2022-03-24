<template>
  <div class="home">
    <form class="form-inline my-2 my-lg-0" @submit.prevent="getData()">
      <input v-model="name" class="form-control mr-sm-2" type="ime" placeholder="Name" aria-label="Name">
      <button type="button" @click="getData()" class="btn btn-primary">Submit</button>
    </form>
    <div>
      <p  v-if="age.age">Age: {{age.age}}</p>
      <p v-if="gender.gender">Gender: {{gender.gender}} with probability of {{(gender.probability*100).toFixed(2)}} %</p>
      <div v-if="nationality.country">Nationality: 
        <p v-for="nation in this.nationality.country" :key="nation" >{{nation.country_id}} with probability of {{(nation.probability*100).toFixed(2)}} %</p>

      </div>
    </div>
  </div>
</template>

<script>

import axios from 'axios';

export default {
  name: 'HomeView',
  data () {
    return {
      name : "",
      age : {},
      gender : {},
      nationality : {}
    }
  },
  methods : {
    getData() {
      axios.get(
        "https://api.agify.io/?name="+this.name
      ).then((data) => {
        this.age = data.data;
      }).catch((error) => {
        console.log(error);
      });

      axios.get(
        "https://api.genderize.io?name="+this.name
      ).then((data) => {
        this.gender = data.data;
      }).catch((error) => {
        console.log(error);
      })

      axios.get(
        "https://api.nationalize.io?name="+this.name
      ).then((data) => {
        this.nationality = data.data;
      }).catch((error) => {
        console.log(error);
      })

    }
  }
}

</script>
