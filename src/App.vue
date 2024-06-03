<template>
  <v-app>
    <v-main>
      <FetchData />
    </v-main>
  </v-app>
</template>

<script>
import FetchData from "./components/FetchData.vue";

export default {
  components: {
    FetchData,
  },
};
</script>
