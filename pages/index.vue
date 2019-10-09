<template>
  <v-row class="justify-space-between">
    <v-col cols="12" md="3">
      <v-autocomplete
        placeholder="Search for a country..."
        v-model="countrySelect"
        :items="countryNameList"
        solo
      />
    </v-col>
    <v-col cols="7" md="2">
      <v-select placeholder="Filter by Region" v-model="regionFilter" :items="regionList" solo />
    </v-col>
    <v-col cols="12">
      <v-row class="justify-center">
        <v-col
          cols="10"
          md="3"
          v-for="(country, index) in displayedCountries"
          :key="'country-card-'+index+'-' +(regionFilter || '') + (countrySelect || '')"
        >
          <v-lazy transition="fade-transition" class="fill-height">
            <v-card @click="toCountryDetails(country)" class="fill-height">
              <v-img :src="country.flag" height="160">
                <template v-slot:placeholder>
                  <v-row class="fill-height ma-0" align="center" justify="center">
                    <v-progress-circular indeterminate color="grey lighten-5"></v-progress-circular>
                  </v-row>
                </template>
              </v-img>
              <v-card-title class="subtitle-1 font-weight-bold my-3">{{country.name}}</v-card-title>
              <v-card-text :class="$vuetify.theme.dark ? 'white--text' : 'black--text'" class="font-weight-thin">
                <p>
                  <b>Population:</b>
                  {{country.population.toLocaleString('en')}}
                </p>
                <p>
                  <b>Region:</b>
                  {{country.region}}
                </p>
                <p>
                  <b>Capital:</b>
                  {{country.capital}}
                </p>
              </v-card-text>
            </v-card>
          </v-lazy>
        </v-col>
      </v-row>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      countries: [],
      regionFilter: null,
      countrySelect: null,
      loading: true
    };
  },
  async created() {
    await this.getCountries();
    this.loading = false;
  },
  methods: {
    async getCountries() {
      this.countries = (await this.$axios.get(
        "https://restcountries.eu/rest/v2/all"
      )).data;
    },
    toCountryDetails(country) {
      this.$router.push({
        name: "alphaCode",
        params: { alphaCode: country.alpha3Code }
      });
    }
  },
  computed: {
    displayedCountries() {
      if (this.countrySelect) {
        return this.countries.filter(c => this.countrySelect == c.alpha3Code);
      }
      if (this.regionFilter) {
        return this.countries.filter(c => c.region === this.regionFilter);
      }
      return this.countries;
    },
    regionList() {
      let regionSet = new Set();
      this.countries.forEach(c => regionSet.add(c.region));
      return Array.from(regionSet);
    },
    countryNameList() {
      let countryList = this.countries.map(c => ({
        text: c.name,
        value: c.alpha3Code
      }));
      countryList.unshift({ text: "All", value: null });
      return countryList;
    }
  }
};
</script>