<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Vuetify</span>
      </v-toolbar-title>
    </v-toolbar>

    <v-content><Form></Form></v-content>
  </v-app>
</template>

<script>
export default {
  name: "App",
  components: {
    Form: () => import(/* webpackChunkName: "Form" */ "./components/Form")
  },
  data() {
    return {
      //
    };
  }
};
</script>
