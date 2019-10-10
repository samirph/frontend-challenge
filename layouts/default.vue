<template>
  <v-app :dark="darkTheme">
    <v-content :class="{darkerBlue: darkTheme}">
      <v-row  class="py-2 elevation-1 justify-center" :class="darkTheme ? 'darkBlue' : 'white'">
        <v-col cols="7" md="6" class="d-flex align-center">
          <h3 class="ml-4 subtitle-1 font-weight-bold">Where in the world?</h3>
        </v-col>
        <v-col cols="5">
          <div class="text-right">
            <v-btn class="text-capitalize" @click="toggleTheme" text>
              <v-icon>nights_stay</v-icon>Dark mode
            </v-btn>
          </div>
        </v-col>
      </v-row>
      <v-container fluid>
        <v-row class="justify-center">
          <v-col cols="11">
            <nuxt />
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      darkTheme: false
    };
  },
  methods: {
    toggleTheme() {
      this.darkTheme = !this.darkTheme;
      this.$vuetify.theme.dark = this.darkTheme;
    }
  }
};
</script>

<style>
.v-application {
  font-family: "Nunito", sans-serif !important;
}
p{
  margin-bottom: 4px !important;
}

.theme--dark.v-text-field--solo > .v-input__control > .v-input__slot, .theme--dark.v-card, .theme--dark.v-list, .v-application .grey.darken-3{
  background-color: #2b3945 !important;
}
</style>