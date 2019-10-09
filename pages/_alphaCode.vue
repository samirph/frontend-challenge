<template>
  <v-row>
    <v-col>
      <v-btn color="white" class="text-capitalize" @click="$router.back()">
        <v-icon>keyboard_arrow_left</v-icon>Back
      </v-btn>
    </v-col>
    <v-col v-if="!loading && country.name" class="font-weight-thin">
      <v-img :src="country.flag" class="mb-12" />
      <h1 class="headline mb-8"><b>{{country.name}}</b></h1>
      <p>
        <b>Native Name:</b>
        {{country.name}}
      </p>
      <p>
        <b>Population:</b>
        {{country.population}}
      </p>
      <p>
        <b>Region:</b>
        {{country.region}}
      </p>
      <p>
        <b>Sub Region:</b>
        {{country.subregion}}
      </p>
      <p>
        <b>Capital:</b>
        {{country.capital}}
      </p>
      <p class="mt-12">
        <b>Top Level Domain:</b>
        {{country.topLevelDomain.join(', ')}}
      </p>
      <p>
        <b>Currencies:</b>
        {{currencies.join(', ')}}
      </p>
      <p>
        <b>Languages:</b>
        {{languages.join(', ')}}
      </p>
    </v-col>
    <v-col v-if="!loading && borderCountries && borderCountries.length > 0">
      <p class="title font-weight-regular">Border Countries</p>
      <v-btn
        :to="{name: 'alphaCode', params: {alphaCode: country.alpha3Code}}"
        v-for="(country, index) in borderCountries"
        class="mr-1 my-1 text-capitalize font-weight-thin"
        :key="'border-country-' + index"
      >{{country.name}}</v-btn>
    </v-col>
    <v-col v-if="loading" cols="12"  class="loader-container d-flex justify-center align-center" >
      <v-progress-circular :size="70" :width="7" color="indigo" indeterminate></v-progress-circular>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      country: {},
      borderCountries: [],
      loading: true
    };
  },
  async created() {
    await this.getCountry();
    if (this.country.borders && this.country.borders.length > 0)
      await this.getBorderCountriesByCode(this.country.borders);
    this.loading = false;
  },
  methods: {
    async getCountry() {
      this.country = (await this.$axios.get(
        "https://restcountries.eu/rest/v2/alpha/" + this.$route.params.alphaCode
      )).data;
    },
    async getBorderCountriesByCode(countryCodes) {
      const codesParam = countryCodes.join(";");
      this.borderCountries = (await this.$axios.get(
        "https://restcountries.eu/rest/v2/alpha/?codes=" + codesParam
      )).data;
    }
  },
  computed: {
    currencies() {
      if (!this.country.currencies) return [];
      return this.country.currencies.map(c => c.name);
    },
    languages() {
      if (!this.country.languages) return [];
      return this.country.languages.map(c => c.name);
    }
  }
};
</script>

<style scoped>
.loader-container{
    height: 600px;
}
</style>