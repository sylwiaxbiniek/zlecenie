<template>
  <v-app>
    <v-app-bar app color="primary" dark absolute>
      <v-toolbar-title>Zlecenie lekowe dla pacjenta</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <v-card mt-5 class="fill-height">
        <v-tabs v-model="tab" centered>
          <v-tab>Formularz</v-tab>
          <v-tab>Historia</v-tab>
        </v-tabs>
        <v-tabs-items v-model="tab">
          <v-tab-item>
            <Formularz @noweZlecenie="noweZlecenie($event)"></Formularz>
          </v-tab-item>
          <v-tab-item>
            <Historia :zlecenia="zlecenia"></Historia>
          </v-tab-item>
        </v-tabs-items>
      </v-card>
    </v-main>
  </v-app>
</template>

<script>
import Formularz from "./components/Formularz";
import Historia from "./components/Historia";

export default {
  name: "App",

  components: {
    Formularz,
    Historia
  },

  methods: {
   noweZlecenie (zlecenie) {
     this.zlecenia.push(zlecenie);
   }
},

  data: () => ({
    tab: null,
    zlecenia: []
  })
};
</script>
