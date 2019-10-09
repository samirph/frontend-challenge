<template>
  <v-app :dark="darkTheme">
    <v-content>
      <v-row class="py-4 elevation-1" :class="darkTheme ? 'grey darken-3' : 'white'">
        <v-col cols="7">
          <h3 class="ml-2 subtitle-1 font-weight-bold">Where in the world?</h3>
        </v-col>
        <v-col cols="5">
          <div class="text-right">
            <v-btn class="text-capitalize" @click="toggleTheme" text><v-icon>nights_stay</v-icon>Dark mode</v-btn>
          </div>
        </v-col>
      </v-row>
      <v-container class="">
        <nuxt />
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      darkTheme: false
    }
  },
  methods: {
    toggleTheme() {
      this.darkTheme = !this.darkTheme
      this.$vuetify.theme.dark = this.darkTheme
    }
  }
};
</script>

<style scoped>
.v-application {
  font-family: 'Nunito', sans-serif !important;
}
</style>