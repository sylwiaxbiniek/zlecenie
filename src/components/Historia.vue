<template>
  <v-container class="my-10">
    <v-simple-table class="table table-hover">
      <thead>
        <tr>
          <th class="text-left">Imię</th>
          <th class="text-left">Nazwisko</th>
          <th class="text-left">PESEL</th>
          <th class="text-left">Oddział</th>
          <th class="text-left">Nazwa Leku</th>
          <th class="text-left">Ilość</th>
          <th class="text-left">Data</th>
          <th class="text-left">Czas</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in zlecenia" v-bind:key="item.imie">
          <td>{{ item.imie }}</td>
          <td>{{ item.nazwisko }}</td>
          <td>{{ item.pesel }}</td>
          <td>{{ item.oddzial }}</td>
          <td>{{ item.lek }}</td>
          <td>{{ item.ilosc }}</td>
          <td>{{ item.data }}</td>
          <td>{{ item.czas }}</td>
        </tr>
      </tbody>
    </v-simple-table>
  </v-container>
</template>


<script>
export default {
  props: ["zlecenia"]
};
</script>