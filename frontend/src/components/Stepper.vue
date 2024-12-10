<template>
  <v-stepper v-model="step" alt-labels :items="['Step 1', 'Step 2', 'Step 3']">
    <!-- Step 1: City Selection -->
    <template v-slot:item.1>
      <v-card title="Step One" flat>
        <v-form ref="stepForm1" v-model="validStep1">
          <SelectCity @input="validateCity" />
          <v-btn :disabled="!validStep1" @click="nextStep">Next</v-btn>
        </v-form>
      </v-card>
    </template>

    <!-- Step 2: Input Field -->
    <template v-slot:item.2>
      <v-card title="Step Two" flat>
        <v-form ref="stepForm2" v-model="validStep2">
          <v-text-field
            label="Enter something for Step 2"
            :rules="step2Rules"
            v-model="step2Input"
            required
          ></v-text-field>
        </v-form>
        <v-btn :disabled="!validStep2" @click="nextStep">Next</v-btn>
        <v-btn @click="previousStep">Back</v-btn>
      </v-card>
    </template>

    <!-- Step 3: Form with Validation -->
    <template v-slot:item.3>
      <v-card title="Step Three" flat>
        <v-form ref="stepForm3" v-model="validStep3">
          <Form @input="validateForm" />
        </v-form>
        <v-btn @click="previousStep">Back</v-btn>
        <v-btn :disabled="!validStep3" @click="finish">Finish</v-btn>
      </v-card>
    </template>
  </v-stepper>
</template>

<script>
import SelectCity from './SelectCity.vue';
import Form from './Form.vue';

export default {
  components: {
    SelectCity,
    Form,
  },
  data() {
    return {
      step: 1,
      step1Input: '',
      step2Input: '',
      step3Input: '',
      validStep1: false,
      validStep2: false,
      validStep3: false,
      city: '',
      step2Rules: [
        v => !!v || 'Step 2: This field is required.',
        v => (v && v.length >= 4) || 'Step 2: Must be at least 4 characters long.',
      ],
      step3Rules: [
        v => !!v || 'Step 3: This field is required.',
        v => (v && v.length >= 4) || 'Step 3: Must be at least 4 characters long.',
      ],
    };
  },
  methods: {
    validateCity(selectedCity) {
      this.city = selectedCity;
      this.validStep1 = !!selectedCity;
    },
    validateForm() {
      // Check validity of Form inputs in Step 3
      const isValid = this.$refs.form.validate();
      this.validStep3 = isValid;
    },
    nextStep() {
      if (this.step === 1 && !this.validStep1) {
        alert('Please select a city before proceeding.');
        return;
      }
      if (this.step === 2 && !this.validStep2) {
        alert('Please complete Step 2 before proceeding.');
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
        this.step2Input = '';
        this.step3Input = '';
        this.validStep1 = false;
        this.validStep2 = false;
        this.validStep3 = false;
      } else {
        alert('Please complete Step 3 before finishing.');
      }
    },
  },
};
</script>

<style scoped>

</style>
