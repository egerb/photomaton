<template >
  <v-stepper id="app" hide-actions v-model="step"  alt-labels :items="['TU EXPERIENCIA\n', 'DATOS PERSONALES\n', 'DATOS DEL EVENTO']">
    <!-- Step 1: City Selection -->
    <template v-slot:item.1>
      <v-card id="app" title="¿Dónde se va a realizar la boda?" flat>
        <v-form  ref="stepForm1" v-model="validStep1">
          <SelectCity @input="validateCity"  />
          <v-btn id="buttons" :disabled="!validStep1" @click="nextStep">Next</v-btn>
        </v-form>
      </v-card>
    </template>

    <!-- Step 2: Updated with Carousel and OptionalProducts validation -->
    <template v-slot:item.2>
      <v-card id="app" title="Elige algo que se adapte a tu evento." flat>
        <OptionProducts
          @select-card="handleCardSelection"
          @selection-changed="handleOptionalProductsChange"
          @update:valid="handleOptionalProductsValid"
        />
        <v-form ref="stepForm2" v-model="validStep2">
        </v-form>
        <v-btn id="buttons" :disabled="!validStep2 || !validOptionalProducts" @click="nextStep">Next</v-btn>
        <v-btn id="buttons" @click="previousStep">Back</v-btn>
      </v-card>
    </template>

    <!-- Step 3: Form with Validation -->
    <template v-slot:item.3>
      <v-card id="app" title="Completa tus datos" flat>
        <v-form ref="formRef" v-model="validStep3" @update:model-value="(value) => (validStep3 = value)">
          <Form @submit="validateForm" />
        </v-form>
        <v-btn id="buttons" @click="previousStep">Back</v-btn>
      </v-card>
    </template>
  </v-stepper>
</template>

<script>
import SelectCity from './SelectCity.vue';
import Form from '@/components/Form.vue';
import Carousel from "@/components/Carousel.vue";
import OptionProducts from "@/components/OptionProducts.vue";

export default {
  components: {
    OptionProducts,
    Carousel,
    SelectCity,
    Form,
  },
  data() {
    return {
      step: 1,
      validStep1: false,
      validStep2: false,
      validStep3: false,
      city: '',
      selectedCard: null,
      validOptionalProducts: false,
      selectedOptionalProducts: [],
    };
  },
  methods: {
    validateCity(selectedCity) {
      this.city = selectedCity;
      this.validStep1 = !!selectedCity;
    },
    validateForm() {
      const validateForm = () => {
        if (formRef.value) {
          const isValid = formRef.value.validate();
          validStep3.value = isValid;
          return isValid;
        } else {
          console.error("Form reference is not available.");
          return false;
        }
      };

    },
    handleCardSelection(valid) {
      this.validStep2 = true;
    },
    handleOptionalProductsChange(selectedItems) {
      this.selectedOptionalProducts = selectedItems;
    },
    handleOptionalProductsValid(isValid) {
      this.validOptionalProducts = isValid;
    },
    nextStep() {
      if (this.step === 1 && !this.validStep1) {
        alert('Please select a city before proceeding.');
        return;
      }
      if (this.step === 2 && (!this.validStep2 || !this.validOptionalProducts)) {
        alert('Please select a photomaton and at least one optional product before proceeding.');
        return;
      }
      if (this.step === 3 && !this.validStep3) {
        alert('Please complete Step 3 before proceeding.');
        return;
      }
      this.step++;
    },
    previousStep() {
      if (this.step > 1) {
        this.step--;
      }
    },
    finish() {
      if (this.validStep3) {
        alert('Form completed!');
        this.step = 1;
        this.step1Input = '';
        this.selectedCard = null;
        this.selectedOptionalProducts = [];
        this.validStep1 = false;
        this.validStep2 = false;
        this.validStep3 = false;
        this.validOptionalProducts = false;
      } else {
        alert('Please complete Step 3 before finishing.');
      }
    },
  },
};
</script>

<style scoped>

</style>
