<template>
  <main v-if="!loading">

    <DataTitle :text="title" :dataDate="dataDate" />
    
    <DataBoxes :stats="stats" />
    <CountrySelect :countries="countries" @get-country="getCountryData" />
    <button
      @click="clearCountryData()"
      v-if="stats.Country"
      class="
        bg-green-700
        p-3
        mt-10
        text-white
        rounded
        focus:outline-none
        hover:bg-green-600
      "
    >
      Clear Country
    </button>
  </main>
  <main v-else class="flex flex-col align-center justify-center text-center">
    <div class="text-gray-500 text-3xl mt-10 mb-6">Fetching Data</div>
  </main>
</template>

<script>
import DataTitle from "../components/DataTitle.vue";
import DataBoxes from "../components/DataBoxes.vue";
import CountrySelect from '../components/CountrySelect.vue';

export default {
  name: "Header",
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
  },
  data() {
    return {
      loading: true,
      title: "Global",
      dataDate: "",
      stats: "",
      countries: [],
      //   loadingImages: require(""),
    };
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch("https://api.covid19api.com/summary");
      const data = await res.json();
      return data;
    },
    getCountryData(country) {
      this.stats = country;
      this.title = country.Country;
    },
    async clearCountryData(){
      this.loading = true;
      const data = await this.fetchCovidData();
      this.stats = data.Global;
      this.title = 'Global';
      this.loading = true;
    }
  },
  async created() {
    const data = await this.fetchCovidData();

    console.log(data);

    this.dataDate = data.Date;
    this.stats = data.Global;
    this.countries = data.Countries;
    this.loading = false;
  },
};
</script>

<style>
</style>