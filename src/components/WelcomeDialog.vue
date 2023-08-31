<template>
  <v-dialog v-model="internalDialog" max-width="500px">
    <v-card>
      <v-card-title class="headline">Bienvenido a la App de Gestión Financiera</v-card-title>
      <v-card-text>
        <v-text-field v-model="nombre" label="Nombre"></v-text-field>
        <v-text-field v-model.number="presupuesto" label="Presupuesto disponible" type="number"></v-text-field>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="primary" @click="empezar">Empezar</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  props: ['dialog'],
  data() {
    return {
      internalDialog: false,
      nombre: '',
      presupuesto: null
    };
  },
  watch: {
    dialog(newVal) {
      this.internalDialog = newVal;
    }
  },
  methods: {
    empezar() {
      if (this.nombre && this.presupuesto !== null) {
        this.$emit('empezar', this.nombre, this.presupuesto);
        this.internalDialog = false; // Cerrar el diálogo después de empezar
      }
    }
  }
};
</script>

