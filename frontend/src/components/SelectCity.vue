<template>
  <v-select
    label="Select a city"
    :items="['Alicante', 'Barcelona', 'Santander', 'Gjon', 'Madrid']"
    variant="outlined"
    :rules="[v => !!v || 'Please enter a valid city.']"
    @change="emitCity"
  ></v-select>
</template>

<script>
export default {
  methods: {
    emitCity(selectedCity) {
      this.$emit('input', selectedCity);
    },
  },
};
</script>
