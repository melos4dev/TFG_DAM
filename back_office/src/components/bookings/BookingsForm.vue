<template>
  <v-card>
    <v-form>
      <v-container>
        <v-row>
          <v-col cols="12" md="4">
            <v-text-field v-model="classroomName" label="Nombre del Aula"></v-text-field>
          </v-col>
          <v-col cols="12" md="4">
            <v-select v-model="type" :items="tipos" :menu-props="{ maxHeight: '400' }" label="Tipo de aula"
              hint="Selecciona el tipo de aula" persistent-hint></v-select>
          </v-col>
          <v-col cols="12" md="4">
            <v-select v-model="zone" :items="zonas" :menu-props="{ maxHeight: '400' }" label="Zona"
              hint="Selecciona la zona del aula" persistent-hint></v-select>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12" md="3">
            <v-text-field v-model="emailUser" label="Profesor"></v-text-field>
          </v-col>
          <v-col cols="12" md="3">
            <v-text-field v-model="studentGroup" label="Grupo alumnos"></v-text-field>
          </v-col>
          <v-col cols="12" md="3">
            <v-text-field v-model="date" label="Fecha reserva"></v-text-field>
          </v-col>
          <v-col cols="12" md="3">
            <v-select v-model="hour" :items="horas" :menu-props="{ maxHeight: '400' }" label="Hora reserva"
              hint="Selecciona horas disponibles" persistent-hint></v-select>
          </v-col>
        </v-row>

        <br />
        <v-btn @click="save()" color="primary">Enviar</v-btn>

      </v-container>
    </v-form>

  </v-card>
</template>

<script>
import BookingsRepository from '@/repository/BookingsRepository.js';
export default {

  data() {
    return {
      classroomName: '',
      type: [],
      zone: [],
      date: '',
      hour: '',
      studentGroup: '',
      emailUser: '',
      horas: ['1ª', '2ª', '3ª', '4ª', '5ª', '6ª'],
      zonas: ['Zona 1', 'Zona 2', 'Zona 3'],
      tipos: ['Aula ordenadores', 'Carro ordenadores', 'Aula simple']
    };
  },
  methods: {
    async save() {
      const nuevoRegistro = {
        classroomName: this.classroomName,
        type: this.type,
        zone: this.zone,
        date: this.date,
        hour: this.hour,
        studentGroup: this.studentGroup,
        emailUser: this.emailUser,

      };

      try {
        await BookingsRepository.save(nuevoRegistro)
        this.$emit("bookingCreate")
      } catch (error) {
        console.log(error)
      }

      this.resetForm();
    },
    resetForm() {
      this.classroomName = '',
        this.type = [],
        this.zone = [],
        this.date = '',
        this.hour = '',
        this.studentGroup = '',
        this.emailUser = ''
    }
  }
};
</script>