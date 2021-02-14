<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <h1>Arbeitsblatt 16-4</h1>
      <v-spacer></v-spacer>
      <v-subheader>by Sandro Gerber</v-subheader>
    </v-app-bar>
    <v-main>
      <rest-api />
    </v-main>
  </v-app>
</template>

<script>
import RestApi from "./components/RestApi.vue";

export default {
  name: "App",

  components: {
    RestApi,
  },

  data: () => ({
    //
  }),
};
</script>
