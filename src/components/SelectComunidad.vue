<template>
    <select @change="onSelect">
      <option disabled value="">Selecciona una Comunidad Autónoma</option>
      <option
        v-for="(nombre, id) in comunidades"
        :key="id"
        :value="id"
      >
        {{ nombre }}
      </option>
    </select>
  </template>
  
  <script>
  export default {
    props: ["comunidades"],
    methods: {
      onSelect(event) {
        this.$emit("select", event.target.value); // Emitir el ID seleccionado
      },
    },
  };
  </script>
  
  