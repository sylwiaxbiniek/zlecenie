<template>
  <v-container>
    <v-form @submit.prevent="submit">
      <v-subheader>Dane pacjenta</v-subheader>
      <v-card-subtitle style="max-width: 1000px" flat class="ma-16 text-xs-center">
        <v-card-text>
          <!-- IMIE -->
          <v-text-field
            v-model="imie"
            :error-messages="imieErrors"
            label="Imię"
            required
            @input="$v.imie.$touch()"
            @blur="$v.imie.$touch()"
          ></v-text-field>

          <!-- NAZWISKO -->
          <v-text-field
            v-model="nazwisko"
            :error-messages="nazwiskoErrors"
            label="Nazwisko"
            required
            @input="$v.nazwisko.$touch()"
            @blur="$v.nazwisko.$touch()"
          ></v-text-field>

          <!-- PESEL -->
          <v-text-field
            v-model="pesel"
            :error-messages="peselErrors"
            label="PESEL"
            required
            @input="$v.pesel.$touch()"
            @blur="$v.pesel.$touch()"
          ></v-text-field>

          <!-- ODDZIAL -->
          <v-select
            v-model="oddzial"
            :items="oddzialy"
            :error-messages="oddzialErrors"
            label="Oddział"
            required
            @change="$v.oddzial.$touch()"
            @blur="$v.oddzial.$touch()"
          ></v-select>
        </v-card-text>
      </v-card-subtitle>

      <v-divider :inset="inset"></v-divider>
      <v-subheader>Informacje o leku</v-subheader>
      <v-card-subtitle style="max-width: 1000px" flat class="ma-16 text-xs-center">
        <v-card-text>
          <!-- NAZWA LEKU -->
          <v-select
            v-model="lek"
            :items="leki"
            :error-messages="lekErrors"
            label="Nazwa leku"
            required
            @change="$v.lek.$touch()"
            @blur="$v.lek.$touch()"
          ></v-select>

          <!-- ILOSC LEKU -->
          <v-text-field
            v-model="ilosc"
            :error-messages="iloscErrors"
            label="Ilość"
            required
            @input="$v.ilosc.$touch()"
            @blur="$v.ilosc.$touch()"
          ></v-text-field>

          <!-- DATA -->
          <v-menu
            ref="menu_data"
            v-model="menu_data"
            :close-on-content-click="false"
            :return-value.sync="data"
            transition="scale-transition"
            offset-y
            min-width="290px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="data"
                :error-messages="dataErrors"
                label="Data podania"
                prepend-icon="event"
                readonly
                v-bind="attrs"
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker v-model="data" no-title scrollable>
              <v-spacer></v-spacer>
              <v-btn depressed color="primary" @click="menu_data = false">wyjdź</v-btn>
              <v-btn depressed color="primary"  @click="$refs.menu_data.save(data)">zapisz</v-btn>
            </v-date-picker>
          </v-menu>

          <!-- CZAS -->
          <v-menu
            ref="menu_czas"
            v-model="menu_czas"
            :close-on-content-click="false"
            :nudge-right="40"
            :return-value.sync="czas"
            transition="scale-transition"
            offset-y
            max-width="290px"
            min-width="290px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="czas"
                :error-messages="czasErrors"
                label="Pora podania"
                prepend-icon="access_time"
                readonly
                v-bind="attrs"
                v-on="on"
              ></v-text-field>
            </template>
            <v-time-picker
              v-if="menu_czas"
              v-model="czas"
              full-width
              @click:minute="$refs.menu_czas.save(czas)"
            ></v-time-picker>
          </v-menu>
        </v-card-text>
      </v-card-subtitle>

      <v-card-actions>
        <v-btn depressed large color="primary" class="mr-4" @click="submit">wyślij</v-btn>
        <v-btn depressed large color="primary" @click="clear">wyczyść</v-btn>
      </v-card-actions>
    </v-form>
  </v-container>
</template>


<script>
import { validationMixin } from "vuelidate";
import { required, minLength, maxLength } from "vuelidate/lib/validators";

export default {
  mixins: [validationMixin],

  validations: {
    imie: { required },
    nazwisko: { required },
    pesel: { required, minLength: minLength(11), maxLength: maxLength(11) },
    oddzial: { required },
    lek: { required },
    ilosc: { required },
    data: { required },
    czas: { required }
  },

  data: () => ({
    imie: null,
    nazwisko: null,
    pesel: null,
    ilosc: null,
    lek: null,
    oddzial: null,
    leki: ["Ibuprom", "Apap", "Rutinoscorbin", "Ketonal"],
    oddzialy: ["Neurochirurgia", "Chirurgia", "Położniczy", "Zakaźny"],

    czas: null,
    menu_czas: false,
    data: null,
    menu_data: false
  }),

  computed: {
    imieErrors() {
      const errors = [];
      if (!this.$v.imie.$dirty) return errors;
      !this.$v.imie.required && errors.push("Pole wymagane");
      return errors;
    },

    nazwiskoErrors() {
      const errors = [];
      if (!this.$v.nazwisko.$dirty) return errors;
      !this.$v.nazwisko.required && errors.push("Pole wymagane");
      return errors;
    },

    peselErrors() {
      const errors = [];
      if (!this.$v.pesel.$dirty) return errors;
      (!this.$v.pesel.maxLength || !this.$v.pesel.minLength) &&
        errors.push("Pesel musi zawierać 11 znaków");
      !this.$v.pesel.required && errors.push("Pole wymagane");
      return errors;
    },

    oddzialErrors() {
      const errors = [];
      if (!this.$v.oddzial.$dirty) return errors;
      !this.$v.oddzial.required && errors.push("Pole wymagane");
      return errors;
    },

    lekErrors() {
      const errors = [];
      if (!this.$v.lek.$dirty) return errors;
      !this.$v.lek.required && errors.push("Pole wymagane");
      return errors;
    },

    iloscErrors() {
      const errors = [];
      if (!this.$v.ilosc.$dirty) return errors;
      !this.$v.ilosc.required && errors.push("Pole wymagane");
      return errors;
    },

    czasErrors() {
      const errors = [];
      if (!this.$v.czas.$dirty) return errors;
      !this.$v.czas.required && errors.push("Pole wymagane");
      return errors;
    },

    dataErrors() {
      const errors = [];
      if (!this.$v.data.$dirty) return errors;
      !this.$v.data.required && errors.push("Pole wymagane");
      return errors;
    }
  },

  methods: {
    submit() {
      console.log("submit!");
      this.$v.$touch();

      if (!this.$v.$invalid) {
        let zlecenie = {
          imie: this.imie,
          nazwisko: this.nazwisko,
          pesel: this.pesel,
          oddzial: this.oddzial,
          lek: this.lek,
          ilosc: this.ilosc,
          data: this.data,
          czas: this.czas
        };
        this.$emit("noweZlecenie", zlecenie);
      }
    },
    clear() {
      this.$v.$reset();
      this.imie = null;
      this.nazwisko = null;
      this.pesel = null;
      this.oddzial = null;
      this.lek = null;
      this.ilosc = null;
      this.czas = null;
      this.menu_czas = false;
      this.data = null;
      this.menu_data = false;
    }
  }
};
</script>