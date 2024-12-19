<template>
    <select @change="onSelect">
      <option disabled value="">Selecciona una Localidad</option>
      <option v-for="localidad in localidades" :key="localidad" :value="localidad">
        {{ localidad }}
      </option>
    </select>
  </template>
  
  <script>
  export default {
    props: ["localidades"],
    methods: {
      onSelect(event) {
        this.$emit("select", event.target.value);
      },
    },
  };
  </script>
  