<template>
  <div class="m-5 row">
    <h2 class="w-100 text-center">FLY.COM</h2>
    <div class="col-12 card">
      <h4>Aeropuertos</h4>
      <table class="table table-primary">
        <thead>
          <tr>
            <th scope="col">id</th>
            <th scope="col">Nombre</th>
            <th scope="col">Lugar</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="aeropuerto in aeropuertos" :key="aeropuerto.id_aeropuerto">
            <th scope="row">{{ aeropuerto.id_aeropuerto }}</th>
            <td>{{ aeropuerto.nombre }}</td>
            <td>{{ aeropuerto.ciudad }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="col-12 card">
      <h4>Aerolineas</h4>
      <table class="table table-success">
        <thead>
          <tr>
            <th scope="col">id</th>
            <th scope="col">Nombre</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="aerolinea in aerolineas" :key="aerolinea.id_aerolinea">
            <th scope="row">{{ aerolinea.id_aerolinea }}</th>
            <td>{{ aerolinea.nombre }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="col-12 card">
      <h4>Reservas</h4>

      <table class="table table-warning">
        <thead>
          <tr>
            <th scope="col">id</th>
            <th scope="col">Aeropuerto Origen</th>
            <th scope="col">Aeropuerto Destino</th>
            <th scope="col">Aerolinea</th>
            <th scope="col">Horario</th>
            <th scope="col">Precio</th>
            <th scope="col">Numero de vuelo</th>
            <th scope="col">TIpo de persona</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="reserva in reservas" :key="reserva.id_vuelo">
            <th scope="row">{{ reserva.id_vuelo }}</th>
            <td>{{ reserva.id_aeropuerto_origen }}</td>
            <td>{{ reserva.id_aeropuerto_destino }}</td>
            <td>{{ reserva.id_aerolinea }}</td>
            <td>{{ reserva.horario }}</td>
            <td>{{ reserva.precio }}</td>
            <td>{{ reserva.numero_vuelo }}</td>
            <td>{{ reserva.tipo_persona }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div>
      <button type="button" class="btn btn-primary" @click="reservarVuelo()">
        Reservar
      </button>
      <form name="form" id="form" v-on:submit.prevent="reservarVuelo()">
        <div class="m-2">
          <label for="">ID Vuelo</label>
          <input type="number" required v-model="id_vuelo" />
        </div>
        <div class="m-2">
          <label for="">Origen</label>
          <input type="number" required v-model="id_aeropuerto_origen" />
        </div>
        <div class="m-2">
          <label for="">Destino</label>
          <input type="number" required v-model="id_aeropuerto_destino" />
        </div>
        <div class="m-2">
          <label for="">Aerolinea</label>
          <input type="number" required v-model="id_aerolinea" />
        </div>
        <div class="m-2">
          <label for="">Horario</label>
          <input type="text" required v-model="horario" />
        </div>
        <div class="m-2">
          <label for="">Precio</label>
          <input type="number" required v-model="precio" />
        </div>
        <div class="m-2">
          <label for="">N- VUelo</label>
          <input type="number" required v-model="numero_vuelo" />
        </div>
        <div class="m-2">
          <label for="">TIpo persona</label>
          <input type="number" required v-model="tipo_persona" />
        </div>
      </form>
    </div>
  </div>
</template>
<script>
export default {
  //Obtener datos
  data() {
    return {
      //Arrays para almacenar los datos de las diferentes tablas
      aeropuertos: [],
      aerolineas: [],
      reservas: [],
      baseURL: "https://aerolinea.herokuapp.com",

      //Par[ametors del formulario
      id_vuelo: 0,
      id_aeropuerto_origen: 0,
      id_aeropuerto_destino: 0,
      id_aerolinea: 0,
      horario: "",
      precio: 0,
      numero_vuelo: 0,
      tipo_persona: 0,
    };
  },
  //llamar datos
  created() {
    this.obtenerAeropuertos();
    this.obtenerAerolineas();
    this.obtenerReservas();
  },

  //crear peticiones
  methods: {
    async obtenerAeropuertos() {
      const res = await this.axios.get(`${this.baseURL}/api/aeropuerto`);
      this.aeropuertos = res.data;
      console.log(res.data);
    },
    async obtenerAerolineas() {
      const res = await this.axios.get(`${this.baseURL}/api/aerolinea`);
      this.aerolineas = res.data;
      console.log(res.data);
    },
    async obtenerReservas() {
      const res = await this.axios.get(`${this.baseURL}/api/vuelo`);
      this.reservas = res.data;
      console.log(res.data);
    },
    async reservarVuelo() {
      const res = await this.axios
        .post(`${this.baseURL}/api/addVuelo`, {
          id_vuelo: this.id_vuelo,
          id_aeropuerto_origen: this.id_aeropuerto_origen,
          id_aeropuerto_destino: this.id_aeropuerto_destino,
          id_aerolinea: this.id_aerolinea,
          horario: this.horario,
          precio: this.precio,
          numero_vuelo: this.numero_vuelo,
          tipo_persona: this.tipo_persona,
        })
        .then(
          (response) => {
            console.log(response);
          },
          (error) => {
            console.log(error);
          }
        );
    },
  },
};
</script>