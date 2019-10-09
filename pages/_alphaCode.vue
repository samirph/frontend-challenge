<template>
  <v-row class="justify-center">
    <v-col cols="12">
      <v-btn :color="$vuetify.theme.dark ? 'grey darken-3' : 'white'" class="text-capitalize" @click="$router.back()">
        <v-icon>keyboard_arrow_left</v-icon>Back
      </v-btn>
    </v-col>
    <v-col v-if="!loading && country.name">
      <v-row class="justify-space-between">
        <v-col cols="12" md="5" class="font-weight-thin">
          <v-img :src="country.flag" class="mb-12" />
        </v-col>
        <v-col cols="12" md="6">
          <v-row>
            <v-col cols="12">
              <h1 class="headline">
                <b>{{country.name}}</b>
              </h1>
            </v-col>
            <v-col cols="12" md="6" class="font-weight-thin">
              <p>
                <b class="font-weight-medium">Native Name:</b>
                {{country.name}}
              </p>
              <p>
                <b class="font-weight-medium">Population:</b>
                {{country.population}}
              </p>
              <p>
                <b class="font-weight-medium">Region:</b>
                {{country.region}}
              </p>
              <p>
                <b class="font-weight-medium">Sub Region:</b>
                {{country.subregion}}
              </p>
              <p>
                <b class="font-weight-medium">Capital:</b>
                {{country.capital}}
              </p>
            </v-col>
            <v-col cols="12" md="6" class="font-weight-thin">
              <p class="mt-12">
                <b class="font-weight-medium">Top Level Domain:</b>
                {{country.topLevelDomain.join(', ')}}
              </p>
              <p>
                <b class="font-weight-medium">Currencies:</b>
                {{currencies.join(', ')}}
              </p>
              <p>
                <b class="font-weight-medium">Languages:</b>
                {{languages.join(', ')}}
              </p>
            </v-col>
            <v-col cols="12" v-if="borderCountries && borderCountries.length > 0">
              <p class="font-weight-regular d-inline-block">Border Countries:</p>
              <br class="hidden-md-and-up" />
              <v-btn
                :to="{name: 'alphaCode', params: {alphaCode: country.alpha3Code}}"
                v-for="(country, index) in borderCountries"
                class="mr-1 my-1 text-capitalize font-weight-regular"
                :color="$vuetify.theme.dark ? 'grey darken-3' : 'white'"
                :key="'border-country-' + index"
              >{{country.name}}</v-btn>
            </v-col>
          </v-row>
        </v-col>
      </v-row>
    </v-col>

    <v-col v-if="loading" cols="12" class="loader-container d-flex justify-center align-center">
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
.loader-container {
  height: 600px;
}
</style>