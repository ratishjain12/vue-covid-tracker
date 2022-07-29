<template>
  <main v-if="!loading">
    <DataTitle :text = "title" :dataDate="dataDate"/>
    <CardBox :stats ="stats"/>
    <CountrySelect class="mb-10" @get-country="getCountryData" :countries = "countries"/>
    <button v-if="stats.Country" @click = "clearCountryData" class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600">
      Clear Country
    </button>
  </main>
  <main v-else class="flex flex-col align-center justify-center text-center">
    <div class="text-gray-500 text-3xl mt-10 mb-6">
        Fetching data
        <img :src="loadingImage" class='w-24 m-auto'/>
    </div>
  </main>
  
</template>

<script>
// @ is an alias to /src

import DataTitle from '../components/DataTitle.vue';
import CardBox from '@/components/CardBox.vue';
import CountrySelect from '@/components/CountrySelect.vue';

export default {
  name: 'HomeView',
  components: {
    DataTitle,
    CardBox,
    CountrySelect,

},
  data(){
    return {
      loading:true,
      title:'Global',
      dataDate:'',
      stats:{},
      countries:[],
      loadingImage:require('../assets/hourglass.gif')
    }
  },
  methods:{
    async fetchCovidData(){
      const res = await fetch('https://api.covid19api.com/summary')
      const data = await res.json();
      return data;
    },
    getCountryData(country){
      this.title = country.Country;
      this.stats = country;
    },
    async clearCountryData(){
      this.loading=true;
      const data = await this.fetchCovidData();
      this.title = 'Global';
      this.stats = data.Global;
      this.loading=false;
    }
  },
  async created(){
    const data = await this.fetchCovidData();
    if(data){
      this.dataDate = data.Date;
      this.stats = data.Global;
      this.countries = data.Countries;
      this.loading = false;
    }else{
      this.loading = true;
    }
    
  }
}
</script>
