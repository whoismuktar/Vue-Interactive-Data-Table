<template>
  <v-app>
    <v-main>
      <!-- <mey-data-table /> -->
      <MelyDataTable />
    </v-main>
  </v-app>
</template>

<script>
import MelyDataTable from "./components/MelyDataTable";

export default {
  name: "App",

  components: {
    MelyDataTable,
  },

  data: () => ({
    //
  }),
};
</script>
