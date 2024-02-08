<template>
  <v-app>
    <v-main>
      <VNet/>
    </v-main>
  </v-app>
</template>

<script>
import VNet from './components/VNet.vue'

export default {
  name: 'App',

  components: {
    VNet,
  },

  data: () => ({
    //
  }),
}
</script>
