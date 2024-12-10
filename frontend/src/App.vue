<template>
  <v-container>
    <Stepper/>
  </v-container>
</template>

<script>
// Import the Stepper component
import Stepper from './components/Stepper.vue';
import SelectCity from "./components/SelectCity.vue";


export default {
  name: 'App',
  components: {
    SelectCity,
    Stepper,

  },
};
</script>

<style>

</style>
