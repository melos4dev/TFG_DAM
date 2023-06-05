<template>
  <v-card>
    <v-form >
    <v-container>
      <v-row>
        <v-col cols="12" md="6">
          <v-text-field v-model="nombre" label="Nombre"></v-text-field>
        </v-col>
        <v-col cols="12" md="6">
          <v-textarea
          v-model="apellido"
          outlined
          name="input-7-4"
          label="Apellido"
          value=""
        ></v-textarea>
     <!--     <v-text-field v-model="apellido" label="Apellido"></v-text-field> -->
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12" md="6">
          <v-select
            v-model="actividad"
            :items="actividades"
            label="Actividad"
          ></v-select>
        </v-col>
        <v-col cols="12" md="6">
          <v-select
          v-model="dia"
          :items="dias"
          :menu-props="{ maxHeight: '400' }"
          label="Dias"
           multiple
           hint="Selecciona tus dias"
          persistent-hint
      ></v-select>

        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12" md="6">
          <v-slider
            v-model="hora"
            :min="0"
            :max="23"
            label="Hora"
            step="1"
            thumb-label
          ></v-slider>
        </v-col>
        <v-col cols="12" md="6">
          <v-radio-group v-model="opcion" row>
            <v-radio label="Efectivo" value="Efectivo"></v-radio>
            <v-radio label="Tarjeta" value="Tarjeta"></v-radio>
            <v-radio label="Bizum" value="opcion3"></v-radio>
          </v-radio-group>
        </v-col>
      </v-row>
      <v-btn @click="save()" color="primary">Enviar</v-btn>
    </v-container>
  </v-form>

  </v-card>
 
</template>

<script>
import pruebaRepository from '@/repository/PruebaRepository.js';
export default {
  
  data() {
    return {
      nombre: '',
      apellido: '',
      actividad: '',
      dia: [],
      hora: 0,
      opcion: '',
      actividades: ['Fútbol', 'Natación', 'Tenis', 'Baloncesto'],
      dias: ['Lunes', 'Martes', 'Miércoles', 'Jueves', 'Viernes']
    };
  },
  methods: {
   async save() {
      const nuevoRegistro = {
        nombre: this.nombre,
        apellido: this.apellido,
        actividad: this.actividad,
        dia: this.dia,
        hora: this.hora,
        opcion: this.opcion
      };

      try {
        await pruebaRepository.save(nuevoRegistro)
        this.$emit("pruebaCreate")
      } catch (error) {
        console.log(error)
      }
    
      this.resetForm();
    },
    resetForm() {
    
      this.nombre = '';
      this.apellido = '';
      this.actividad = '';
      this.dia = [];
      this.hora = 0;
      this.opcion = '';
    }
  }
};
</script>