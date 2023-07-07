<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div class="container">
    <h3>Consulta Covid por Estados</h3>
    <div>
      <ul class="tablaEstados">
        <li v-for="(estado, indice) in codigoEstados" :key="indice">
          {{ estado }}
        </li>
      </ul>
    </div>

    <p >Ingrese los codigos en minusculas</p>

    <input
      type="text"
      class="buscar"
      v-on:keyup.enter="consultar"
      v-model="codigo"
      placeholder="Ingrese codigo de estado"
    />
    <div class="consulta" v-show="consulta">
      <div class="consulta-item">
        <label for="">Casos positivos:</label>
        <input
          type="text"
          readonly
          onmousedown="return false;"
          :value="datos.positive"
        />
      </div>
      <div class="consulta-item">
        <label for="">Total de resultados de pruebas:</label>
        <input
          type="text"
          readonly
          onmousedown="return false;"
          :value="datos.totalTestResults"
        />
      </div>
      <div class="consulta-item">
        <label for="">Actualmente hospitalizados:</label>
        <input
          type="text"
          readonly
          onmousedown="return false;"
          :value="datos.hospitalizedCurrently"
        />
      </div>
      <div class="consulta-item">
        <label for="">Muertos:</label>
        <input
          type="text"
          readonly
          onmousedown="return false;"
          :value="datos.death"
        />
      </div>
      <div class="consulta-item">
        <label for="">Casos positivos virales:</label>
        <input
          type="text"
          readonly
          onmousedown="return false;"
          :value="datos.totalTestsViral"
        />
      </div>
      <div class="consulta-item">
        <label for="">Número de aumento en muertes:</label>
        <input
          type="text"
          readonly
          onmousedown="return false;"
          :value="datos.deathIncrease"
        />
      </div>
    </div>
  </div>
</template>




<script>
export default {
  data() {
    return {
      codigoEstados: null,
      codigo: "",
      consulta: false,
      datos: {
        positive: "",
        totalTestResults: "",
        hospitalizedCurrently: "",
        death: "",
        totalTestsViral: "",
        deathIncrease: "",
      },
    };
  },

  methods: {
    async consultar() {
      const estado = await this.APIConsulta(this.codigo);
      this.datos = estado;
      this.consulta = true;
    },
    async APIListarCodigosEstado() {
      const estados = await fetch(
        "https://api.covidtracking.com/v1/states/current.json"
      ).then((r) => r.json());
      const codigoEstados = [];
      for (let i = 0; i <= 55; i++) {
        codigoEstados.push(estados[i].state);
      }
      this.codigoEstados = codigoEstados;
    },
    async APIConsulta(codigoEstado) {
      const estado = await fetch(
        `https://api.covidtracking.com/v1/states/${codigoEstado}/current.json`
      ).then((r) => r.json());
      const consulta = {
        positive: estado.positive,
        totalTestResults: estado.totalTestResults,
        hospitalizedCurrently: estado.hospitalizedCurrently,
        death: estado.death,
        totalTestsViral: estado.totalTestsViral,
        deathIncrease: estado.deathIncrease,
      };
      return consulta;
    },
  },
  mounted() {
    this.APIListarCodigosEstado();
  },
};
</script>

    
<style scoped>
.consulta label,
input {
  font-size: large;
  display: block;
  text-align: left;
  border-radius: 5px;
  border: solid 1px black;
}

h3 {
    padding: 15px;
}
ul {
  list-style: none;
}

.tablaEstados {
  display: grid;
  grid-template-columns: auto auto auto auto;
  background-color: #fff0ff;
  color: #3e5c8c;
  width: 200px;
  margin: auto;
  padding: 10px;
}

.buscar {
  padding: 10px;
  margin: 30px auto;
}

.consulta {
  background-color: #dfe6e9;
  width: 30vw;
  margin: auto;
  border-radius: 15px;
  padding: 5px 0px;
}

.consulta-item {
  color: black;
  margin: 30px;
}

.consulta label {
  border: 0px;
}
</style>
