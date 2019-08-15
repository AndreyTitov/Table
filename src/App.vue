<template>
  <v-app>
    <v-content>
      <Index/>
    </v-content>
  </v-app>
</template>

<script>
import Index from './views/Index.vue';

export default {
  name: 'App',
  components: {
    Index,
  },
};
</script>
