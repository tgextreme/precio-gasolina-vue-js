<template>
    <div>
      <h1>Precios de Gasolina</h1>
      <table>
        <thead>
          <tr>
            <th>Estación</th>
            <th>Municipio</th>
            <th>Precio Gasolina 95</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="estacion in estaciones" :key="estacion.id">
            <td>{{ estacion.rotulo }}</td>
            <td>{{ estacion.municipio }}</td>
            <td>{{ estacion["Precio Gasolina 95 E5"] }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  
  export default {
    name: "PreciosGasolina",
    data() {
      return {
        estaciones: [],
      };
    },
    mounted() {
      this.obtenerPrecios();
    },
    methods: {
      async obtenerPrecios() {
        try {
          // Cargar el archivo JSON desde la carpeta public
          const respuesta = await axios.get("/estaciones.json");
          this.estaciones = respuesta.data.ListaEESSPrecio;
        } catch (error) {
          console.error("Error al cargar el archivo JSON:", error);
        }
      },
    },
  };
  </script>
  
  <style>
  table {
    border-collapse: collapse;
    width: 100%;
  }
  
  th, td {
    border: 1px solid #ddd;
    padding: 8px;
  }
  
  th {
    background-color: #f4f4f4;
  }
  </style>
  
  