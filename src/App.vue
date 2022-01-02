<template>
  <v-app>
    <v-main>
      <MainApp />
    </v-main>
  </v-app>
</template>

<script>
import MainApp from './components/MainApp';

export default {
  name: 'App',

  components: {
    MainApp,
  },

  data: () => ({
    //
  }),
};
</script>
