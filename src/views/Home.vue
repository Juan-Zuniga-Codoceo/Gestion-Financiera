
<template>
  <div>
    <v-dialog v-model="mostrarBienvenida" max-width="400">
      <v-card>
        <v-card-title class="headline ">Bienvenid@ =)</v-card-title>
        <v-card-text>
          <v-text-field v-model="nombreUsuario" label="Nombre"></v-text-field>
          <v-text-field v-model.number="presupuestoInicialTemp" label="Presupuesto inicial" type="number"></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" @click="iniciarApp">Empezar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-container>
      <v-row>
        <!-- Tarjeta de Gastos -->
        <v-col cols="6">
          <v-card class="card-gastos">
            <v-card-title>Gastos</v-card-title>
            <v-card-text>
              <ul>
                <li v-for="gasto in gastos" :key="gasto.id">
                  {{ gasto.nombre }}: ${{ gasto.cantidad }} ({{ gasto.categoria }})
                  <v-icon small @click="eliminarGasto(gasto)">mdi-delete</v-icon>
                </li>
              </ul>
            </v-card-text>
            <v-card-actions>
              <v-text-field v-model="nuevoGasto.nombre" label="Nombre"></v-text-field>
              <v-text-field v-model.number="nuevoGasto.cantidad" label="Costo $" type="number"></v-text-field>
              <v-select v-model="nuevoGasto.categoria" :items="categorias" label="Categoría"></v-select>
              <v-btn small outline fab color="#d62957" class="add-button" @click="agregarGasto">
              <v-icon small>mdi-plus</v-icon>
              </v-btn>
            </v-card-actions>
            <v-card-actions>
              Total de Gastos: ${{ totalGastos }}
            </v-card-actions>
          </v-card>
        </v-col>
 <!-- Tarjeta de Presupuesto -->
        <v-col cols="3" v-if="presupuestoInicial">
          <v-card class="card-presupuestos">
            <v-card-title>Presupuesto</v-card-title>
            <v-card-text>
              Presupuesto inicial: ${{ presupuestoInicial }}<br>
              Presupuesto restante: ${{ presupuestoRestante }}
            </v-card-text>
          </v-card>
        </v-col>

        <!-- Tarjeta de Gastos Clasificados -->
        <v-col cols="3">
          <v-card class="card-gastos-clasificados">
            <v-card-title>Gastos Clasificados</v-card-title>
            <v-card-text>
              <ul>
                <li v-for="(total, categoria) in gastosClasificados" :key="categoria">{{ categoria }}: ${{ total }}</li>
              </ul>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  name: 'Home-view',
  data() {
    return {
      mostrarBienvenida: true,
      presupuestoInicialTemp: null,
      presupuestoInicial: null,
      nombreUsuario: '',
      gastos: [], // Lista de gastos
      nuevoGasto: { nombre: '', cantidad: null, categoria: null },
      categorias: ['Salud', 'Educación', 'Casa', 'Comida', 'Transporte', 'Ropa', 'Aseo', 'Ocio', 'Mascotas', 'Otros'],
    };
  },
  computed: {
    presupuestoRestante() {
      if (this.presupuestoInicial !== null) {
        const totalGastos = this.gastos.reduce((total, gasto) => total + gasto.cantidad, 0);
        return this.presupuestoInicial - totalGastos;
      }
      return null;
    },
    totalGastos() {
      return this.gastos.reduce((total, gasto) => total + gasto.cantidad, 0);
    },
    gastosClasificados() {
      const categoriasTotales = {};
      this.gastos.forEach(gasto => {
        if (categoriasTotales[gasto.categoria]) {
          categoriasTotales[gasto.categoria] += gasto.cantidad;
        } else {
          categoriasTotales[gasto.categoria] = gasto.cantidad;
        }
      });
      return categoriasTotales;
    }
  },
  methods: {
    agregarGasto() {
      if (this.nuevoGasto.nombre && this.nuevoGasto.cantidad !== null && this.nuevoGasto.categoria) {
        this.gastos.push({ ...this.nuevoGasto });
        this.nuevoGasto.nombre = '';
        this.nuevoGasto.cantidad = null;
        this.nuevoGasto.categoria = null;
      }
    },
    eliminarGasto(gasto) {
      const index = this.gastos.indexOf(gasto);
      if (index !== -1) {
        this.gastos.splice(index, 1);
      }
    },
    iniciarApp() {
      if (this.nombreUsuario && this.presupuestoInicialTemp !== null && this.presupuestoInicialTemp >= 0) {
        this.presupuestoInicial = parseFloat(this.presupuestoInicialTemp);
        this.mostrarBienvenida = false;
      } else {
        alert('Ingresa un nombre y un presupuesto válido mayor o igual a 0.');
      }
    }
  }
};
</script>
<style>
/* Establecer el color de fondo solo para esta vista */
.home-container {
  background-color: #6b9795;
  /* Otros estilos si es necesario */
}
.add-button {
  transition: background-color 0.3s, color 0.3s; /* Transiciones suaves */
  font-size: 18px; /* Tamaño de fuente más pequeño */
  padding: 6px;
}

.add-button:hover {
  background-color: #d62957; /* Color de fondo al pasar el mouse */
  color: white; /* Color del icono al pasar el mouse */
}

.card-gastos {
  box-shadow: rgba(151, 14, 14, 0.3) 0px 19px 38px, rgba(255, 0, 0, 0.22) 0px 15px 12px;
}

.card-presupuestos {
  box-shadow: rgba(121, 234, 121, 0.3) 0px 19px 38px, rgba(0, 255, 0, 0.22) 0px 15px 12px;
}

.card-gastos-clasificados {
  box-shadow: rgba(246, 246, 3, 0.3) 0px 19px 38px, rgba(255, 255, 0, 0.22) 0px 15px 12px;
}

</style>



