<template>

  <v-container>
    <Stepper/>
  </v-container>
</template>

<script>
// Import the Stepper component
import Stepper from './components/Stepper.vue';
import SelectCity from "./components/SelectCity.vue";
import Form from "./components/Form.vue";
import Carousel from "./components/Carousel.vue";
import OptionProducts from "./components/OptionProducts.vue";


export default {
  name: 'App',
  components: {
    SelectCity,
    Stepper,
    Form,
    Carousel,
    OptionProducts,

  },
};
</script>

<style>
#app {
  color: #03162b;
  background-color: white;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
#buttons {
  color: black;
  background-color: #79e3d6;
  margin-top: 10px;
  margin-right: 10px;
}

.v-stepper-item {
  padding: 1.5rem 0 1.5rem 0;
  max-width: 125px;
}

body{
  background-color: white;
}
</style>
