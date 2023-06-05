<template>
    <v-card>
        <v-card-title>
            <v-text-field v-model="search" append-icon="mdi-magnify" label="Buscar" single-line hide-details></v-text-field>
            <div class="text-center">
                <v-btn class="mx-2" fab dark color="#F57C00" @click="dialogCreate = true">
                    <v-icon dark>mdi-plus</v-icon>
                </v-btn>
            </div>
        </v-card-title>
        <v-data-table :headers="headers" :items="bookings" :search="search" :dense="true">
            <template v-slot:item.actions="{ item }">
                <v-icon small color="blue" @click="openUpdateDialog(item)">mdi-pencil</v-icon>
                <v-icon small class="mr-2" color="red" @click="confirmDelete(item)">mdi-delete</v-icon>
            </template>
        </v-data-table>
        <v-dialog v-model="dialogUpdate">
            <v-card>
                <v-form>

                    <v-container>
                        <v-row>
          <v-col cols="12" md="4">
            <v-text-field v-model="selectedItem && selectedItem.classroomName" label="Nombre del Aula"></v-text-field>
          </v-col>
          <v-col cols="12" md="4">
            <v-select v-model="selectedItem && selectedItem.type" :items="tipos" :menu-props="{ maxHeight: '400' }" label="Tipo de aula"
              hint="Selecciona el tipo de aula" persistent-hint></v-select>
          </v-col>
          <v-col cols="12" md="4">
            <v-select v-model="selectedItem && selectedItem.zone" :items="zonas" :menu-props="{ maxHeight: '400' }" label="Zona"
              hint="Selecciona la zona del aula" persistent-hint></v-select>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12" md="3">
            <v-text-field v-model="selectedItem && selectedItem.emailUser" label="Profesor"></v-text-field>
          </v-col>
          <v-col cols="12" md="3">
            <v-text-field v-model="selectedItem && selectedItem.studentGroup" label="Grupo alumnos"></v-text-field>
          </v-col>
          <v-col cols="12" md="3">
            <v-text-field v-model="selectedItem && selectedItem.date" label="Fecha reserva"></v-text-field>
          </v-col>
          <v-col cols="12" md="3">
            <v-select v-model="selectedItem && selectedItem.hour" :items="horas" :menu-props="{ maxHeight: '400' }" label="Hora reserva"
              hint="Selecciona horas disponibles" persistent-hint></v-select>
          </v-col>
        </v-row>
                        <v-btn @click="confirmUpdate(selectedItem)" color="primary">Enviar</v-btn>
                    </v-container>
                </v-form>
            </v-card>
        </v-dialog>

        <v-dialog v-model="dialogCreate">
            <bookingsForm @bookingCreate=create />
        </v-dialog>

    </v-card>
</template>


<script>


import bookingsRepository from '@/repository/BookingsRepository';
import bookingsForm from '@/components/bookings/BookingsForm.vue'


export default {
    data() {
        return {
            selectedItem: null,
            dialogUpdate: false,
            dialogCreate: false,
            search: '',
            headers: [
                {
                    text: 'Nombre del aula',
                    align: 'start',
                    //       filterable: false,
                    value: 'classroomName',
                },
                { text: 'Tipo de aula', value: 'type' },
                { text: 'Zona', value: 'zone' },
                { text: 'Profesor', value: 'emailUser' },
                { text: 'Grupo alumnos', value: 'studentGroup' },
                { text: 'Fecha reserva', value: 'date' },
                { text: 'Hora reserva', value: 'hour' },
                { text: 'Acciones', value: 'actions' }

            ],
            bookings: [
            ],
            horas: ['1ª', '2ª', '3ª', '4ª', '5ª', '6ª'],
            zonas: ['Zona 1', 'Zona 2', 'Zona 3'],
            tipos: ['Aula ordenadores', 'Carro ordenadores', 'Aula simple']
        }
    },
    created() {
        this.get()
    },

    components: {
        bookingsForm
    },

    methods: {

        async get() {
            try {

                this.bookings = await bookingsRepository.getAll()
            }

            catch (error) {
                console.error(error)
            }
        },

        async delete(item) {

            try {
                await bookingsRepository.delete(item)
                this.get()
            }

            catch (error) {
                console.error(error)
            }

        },

        confirmDelete(item) {
            if (confirm("Estas seguro de eliminar")) {
                this.delete(item)
            }
        },

        async update(item) {
            try {
                await bookingsRepository.upDate(item)
                this.get()
            }

            catch (error) {
                console.error(error)
            }
        },

        confirmUpdate(item) {
            if (confirm("Estas seguro de actualizar")) {
                this.update(item)
                this.dialogUpdate = false
            }
        },
        openUpdateDialog(item) {
            this.selectedItem = { ...item }
            this.dialogUpdate = true
        },
        create() {
            this.dialogCreate = false
            this.get()
        }
    }
}
</script>