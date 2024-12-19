<template>
  <div id="app" class="container my-5">
    <h1 class="text-center mb-4">Precios de Combustible</h1>

    <!-- Selección de Provincia -->
    <div class="mb-3">
      <label for="provincia" class="form-label">Provincia:</label>
      <select
        id="provincia"
        class="form-select"
        @change="onProvinciaChange"
        v-model="provinciaSeleccionada"
      >
        <option value="">Seleccione una Provincia</option>
        <option v-for="provincia in provincias" :key="provincia" :value="provincia">
          {{ provincia }}
        </option>
      </select>
    </div>

    <!-- Selección de Localidad -->
    <div class="mb-3" v-if="localidades.length">
      <label for="localidad" class="form-label">Localidad:</label>
      <select
        id="localidad"
        class="form-select"
        @change="onLocalidadChange"
        v-model="localidadSeleccionada"
      >
        <option value="">Seleccione una Localidad</option>
        <option v-for="localidad in localidades" :key="localidad" :value="localidad">
          {{ localidad }}
        </option>
      </select>
    </div>

    <!-- Selección de Tipo de Combustible -->
    <div class="mb-3" v-if="localidadSeleccionada">
      <label for="combustible" class="form-label">Tipo de Combustible:</label>
      <select
        id="combustible"
        class="form-select"
        v-model="combustibleSeleccionado"
      >
        <option value="">Seleccione un Tipo</option>
        <option v-for="combustible in tiposCombustible" :key="combustible" :value="combustible">
          {{ combustible }}
        </option>
      </select>
    </div>

    <!-- Tabla de Precios -->
    <div v-if="resultadoOrdenado.length">
      <h2 class="text-center mt-4 mb-3">Estaciones en {{ localidadSeleccionada }}</h2>
      <table class="table table-striped table-bordered">
        <thead class="table-dark">
          <tr>
            <th scope="col">Nombre de Gasolinera</th>
            <th scope="col">Dirección</th>
            <th scope="col">Precio ({{ combustibleSeleccionado }})</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="estacion in resultadoOrdenado" :key="estacion.IDEESS">
            <td>{{ estacion.Rótulo }}</td>
            <td>{{ estacion.Dirección }}</td>
            <td>{{ estacion[combustibleSeleccionado] || "N/A" }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      estaciones: [], // Datos de las estaciones
      provincias: [], // Lista de provincias únicas
      localidades: [], // Localidades según la provincia seleccionada
      provinciaSeleccionada: "",
      localidadSeleccionada: "",
      tiposCombustible: [
        "Precio Gasolina 95 E5",
        "Precio Gasolina 98 E5",
        "Precio Gasoleo A",
        "Precio Gasoleo Premium",
      ],
      combustibleSeleccionado: "",
      resultado: [], // Estaciones filtradas por localidad
    };
  },
  computed: {
    resultadoOrdenado() {
      // Ordenar por precio de combustible de más barato a más caro
      return this.resultado
        .filter((estacion) => estacion[this.combustibleSeleccionado]) // Filtrar estaciones con precio válido
        .sort((a, b) => a[this.combustibleSeleccionado] - b[this.combustibleSeleccionado]);
    },
  },
  async created() {
    await this.cargarDatos();
  },
  methods: {
    async cargarDatos() {
      const response = await fetch("/estaciones.json");
      const data = await response.json();

      this.estaciones = data.ListaEESSPrecio;

      // Extraer provincias únicas
      this.provincias = [
        ...new Set(this.estaciones.map((estacion) => estacion.Provincia)),
      ].sort();
    },
    onProvinciaChange() {
      this.localidadSeleccionada = "";
      this.resultado = [];

      // Filtrar localidades por provincia seleccionada
      this.localidades = [
        ...new Set(
          this.estaciones
            .filter((estacion) => estacion.Provincia === this.provinciaSeleccionada)
            .map((estacion) => estacion.Localidad)
        ),
      ].sort();
    },
    onLocalidadChange() {
      // Filtrar estaciones por localidad seleccionada
      this.resultado = this.estaciones.filter(
        (estacion) => estacion.Localidad === this.localidadSeleccionada
      );
    },
  },
};
</script>






<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
