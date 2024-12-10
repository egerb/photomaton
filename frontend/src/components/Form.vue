<template>
  <form @submit.prevent="submit">
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

    <v-select
      v-model="form.select"
      :error-messages="errors.select"
      :items="items"
      label="Select"
    ></v-select>

    <v-checkbox
      v-model="form.checkbox"
      :error-messages="errors.checkbox"
      label="Option"
      value="1"
    ></v-checkbox>

    <v-btn
      class="me-4"
      type="submit"
    >
      Submit
    </v-btn>

    <v-btn @click="resetForm">
      Clear
    </v-btn>
  </form>
</template>

<script setup>
import { reactive, ref } from 'vue'

// Form state
const form = reactive({
  name: '',
  phone: '',
  email: '',
  select: null,
  checkbox: null,
})

// Error messages
const errors = reactive({
  name: '',
  phone: '',
  email: '',
  select: '',
  checkbox: '',
})

// Dropdown items
const items = ref(['Item 1', 'Item 2', 'Item 3', 'Item 4'])

// Validation logic
const validate = () => {
  let isValid = true

  // Name validation
  if (form.name.length < 2) {
    errors.name = 'Name needs to be at least 2 characters.'
    isValid = false
  } else {
    errors.name = ''
  }

  // Phone validation
  if (!/^[0-9-]{7,}$/.test(form.phone)) {
    errors.phone = 'Phone number needs to be at least 7 digits.'
    isValid = false
  } else {
    errors.phone = ''
  }

  // Email validation
  if (!/^[a-z.-]+@[a-z.-]+\.[a-z]+$/i.test(form.email)) {
    errors.email = 'Must be a valid e-mail.'
    isValid = false
  } else {
    errors.email = ''
  }

  // Select validation
  if (!form.select) {
    errors.select = 'Select an item.'
    isValid = false
  } else {
    errors.select = ''
  }

  // Checkbox validation
  if (form.checkbox !== '1') {
    errors.checkbox = 'Must be checked.'
    isValid = false
  } else {
    errors.checkbox = ''
  }

  return isValid
}

// Submit handler
const submit = () => {
  if (validate()) {
    alert('Form submitted successfully: ' + JSON.stringify(form, null, 2))
    resetForm()
  } else {
    alert('Please fix the errors before submitting.')
  }
}

// Reset form
const resetForm = () => {
  form.name = ''
  form.phone = ''
  form.email = ''
  form.select = null
  form.checkbox = null

  // Clear errors
  for (const key in errors) {
    errors[key] = ''
  }
}
</script>
