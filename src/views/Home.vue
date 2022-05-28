<template>
  <main v-if="!loading">
      <DataTitle :text="title" :dataDate="dataDate"></DataTitle>
      <DataBoxes :status="status" ></DataBoxes>

      <CounterySelect @get-country="getCountryData" :countries="countries"></CounterySelect>

      <button @click="clearCountryData" class="bg-green-700 text-white rounded p-3 mt-5 focus:outline-none hover:bg-green-600" v-if="status.Country">Clear Country</button>
  </main>
  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
    <img :src="loadingImage" class="w-24 m-auto" alt="">
  </main>
</template>

<script>


import CounterySelect from '../components/CounterySelect'
import DataBoxes from '../components/DataBoxes'
import DataTitle from '../components/DataTitle'
export default {
  name: 'Home',
  components: {
    CounterySelect,
    DataBoxes,
    DataTitle,
   
  },
  data(){
    return{
      loading:true,
      title:'Global',
      dataDate:'',
      status:{},
      countries:[],
      loadingImage:require('../assets/hourglass.gif'),
    }
  },
  methods:{
    async fetchCovidData() {
      const res = await fetch('https://api.covid19api.com/summary')
      const data = await res.json()
      return data
    },
    getCountryData(country){
      this.status = country
      this.title = country.Country
    },
    async clearCountryData(){
      this.loading = true
      const data = await this.fetchCovidData()
      this.title = "Global"
      this.status = data.Global
      this.loading = false
    }
  },
  async created(){
    const data = await this.fetchCovidData()
    console.log(data)
    this.dataDate = data.Date
    this.status = data.Global
    this.countries = data.Countries
    this.loading = false
  }
}
</script>
