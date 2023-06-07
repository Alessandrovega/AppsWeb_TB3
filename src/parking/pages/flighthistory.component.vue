<template>
  <div class="table-wrapper">
    <table>
      <thead>
      <tr>
        <th>ID</th>
        <th>Origin</th>
        <th>Destination</th>
        <th>Date of Trip</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="trip in trips" :key="trip.id">
        <td>{{ trip.id }}</td>
        <td>{{ trip.origin }}</td>
        <td>{{ trip.destination }}</td>
        <td>{{ trip.date }}</td>
      </tr>
      </tbody>
    </table>
  </div>
  <div>
      <div class="button-container">
          <button class="large-button" @click="dialog = true">Mostrar Diálogo</button>
      </div>
      <div v-if="dialog" class="dialog">
          <div class="dialog-content">
              <h2 style="color: #673ab7">Ingrese información</h2>

             <!--<input type="text" v-model="textbox1" placeholder="Ej.Lima"/>
              <input type="text" v-model="textbox2" placeholder="Ej.Casa de Maco"/>
              <input type="date" v-model="date" placeholder="DD/MM/YYYY"/>-->
              <input v-model="nombre" placeholder="Ingresa tu nombre">
              <div>
                  <button @click="dialog = false">Cerrar</button>
                  <button @click="guardar">Guardar</button>
              </div>
          </div>
      </div>
  </div>
</template>

<script>
import { ref } from 'vue'
export default {

  data() {
    return {
      trips: [
        { id: 1, origin: 'New York', destination: 'Los Angeles', date: '2022-01-01' },
        { id: 2, origin: 'San Francisco', destination: 'Seattle', date: '2022-01-15' },
        { id: 3, origin: 'Chicago', destination: 'Miami', date: '2022-02-01' },
        { id: 4, origin: 'Boston', destination: 'Washington D.C.', date: '2022-02-15' },
      ],
        dialog: false,
        textbox1: '',
        textbox2: '',
        date: '',

    };
  },
    setup() {
        const nombre = ref('')

        function guardar() {
            fetch('http://localhost:3000/nombres', {
                method: 'POST',
                body: JSON.stringify({
                    nombre: nombre.value
                }),
                headers: {
                    'Content-Type': 'application/json'
                }
            })
                .then(response => response.json())
                .then(data => console.log(data))
                .catch((error) => {
                    console.error('Error:', error);
                });
        }

        return {
            nombre,
            guardar
        }
    }





};
</script>

<style scoped>
.table-wrapper {
  background-color: #2196F3; /* color de fondo azul */
  border-radius: 10px; /* bordes redondeados */
  padding: 20px; /* margen interno */
  display: flex;
  justify-content: center;
}

table {
  color: white;
  font-size: 1.2em;
  text-align: left;
  width: 100%;
}

th,
td {
  padding: 10px;
}
.button-container {
    text-align: right;
}

.large-button {
    padding: 10px 20px;
    font-size: 20px;
    background-color: #673ab7;
    color: white;
    border: none;
    border-radius: 5px;
}
th {
  background-color: rgba(255, 255, 255, 0.2);
  font-weight: bold;
  text-transform: uppercase;
}
.dialog {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
}

.dialog-content {
    background: white;
    padding: 20px;
    width: 80%;
    max-width: 400px;
}
</style>