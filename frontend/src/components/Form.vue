<template>
  <form ref="formRef" id="app" @submit.prevent="submit">
    <v-text-field
      v-model="form.name"
      :counter="10"
      :error-messages="errors.name"
      label="Name"
    ></v-text-field>

    <v-text-field
      v-model="form.phone"
      :counter="7"
      :error-messages="errors.phone"
      label="Phone Number"
    ></v-text-field>

    <v-text-field
      v-model="form.email"
      :error-messages="errors.email"
      label="E-mail"
    ></v-text-field>

    <v-text-field
      v-model="form.city"
      :counter="10"
      :error-messages="errors.city"
      label="City"
    ></v-text-field>

    <v-text-field
      v-model="form.address"
      :counter="20"
      :error-messages="errors.address"
      label="Address"
    ></v-text-field>

    <v-checkbox
      v-model="form.checkbox"
      :error-messages="errors.checkbox"
      label="Acepto condiciones y política de datos."
      value="1"
    ></v-checkbox>

    <!-- Success Alert -->
    <v-alert
      v-if="showSuccess"
      type="success"
      variant="outlined"
    >
      Thank you! Your data was saved!
    </v-alert>

    <v-btn
      id="buttons"
      class="me-4"
      type="submit"
    >
      Submit
    </v-btn>

    <v-btn id="buttons" @click="resetForm">
      Clear
    </v-btn>
  </form>
</template>

<script setup>
import { reactive, ref } from 'vue';

const formRef = ref(null);
const showSuccess = ref(false);

// Form state
const form = reactive({
  name: '',
  phone: '',
  email: '',
  city: '',
  address: '',
  checkbox: false,
});

// Error messages
const errors = reactive({
  name: '',
  phone: '',
  email: '',
  city: '',
  address: '',
  checkbox: '',
});

// Validation logic
const validateForm = () => {
  let isValid = true;

  // Name validation
  if (form.name.length < 2) {
    errors.name = 'Name needs to be at least 2 characters.';
    isValid = false;
  } else {
    errors.name = '';
  }

  // Phone validation
  if (!/^[0-9-]{7,}$/.test(form.phone)) {
    errors.phone = 'Phone number needs to be at least 7 digits.';
    isValid = false;
  } else {
    errors.phone = '';
  }

  // Email validation
  if (!/^[a-z.-]+@[a-z.-]+\.[a-z]+$/i.test(form.email)) {
    errors.email = 'Must be a valid e-mail.';
    isValid = false;
  } else {
    errors.email = '';
  }

  // City validation
  if (form.city.length < 2) {
    errors.city = 'City name must be at least 2 characters.';
    isValid = false;
  } else {
    errors.city = '';
  }

  // Address validation
  if (form.address.length < 5) {
    errors.address = 'Address must be at least 5 characters.';
    isValid = false;
  } else {
    errors.address = '';
  }

  // Checkbox validation
  if (!form.checkbox) {
    errors.checkbox = 'You must accept the terms.';
    isValid = false;
  } else {
    errors.checkbox = '';
  }

  return isValid;
};

// Submit handler
const submit = () => {
  if (validateForm()) {
    showSuccess.value = true;
    resetForm();
  } else {
    alert('Please fix the errors before submitting.');
  }
};

// Reset form
const resetForm = () => {
  form.name = '';
  form.phone = '';
  form.email = '';
  form.city = '';
  form.address = '';
  form.checkbox = false;

  // Clear errors
  for (const key in errors) {
    errors[key] = '';
  }

};
</script>
