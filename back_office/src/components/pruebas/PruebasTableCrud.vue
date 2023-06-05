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
        <v-data-table :headers="headers" :items="pruebas" :search="search">
            <template v-slot:item.actions="{ item }">
                <v-icon small class="mr-2" color="red" @click="confirmDelete(item)">mdi-delete</v-icon>
                <v-icon small color="blue" @click="openUpdateDialog(item)">mdi-pencil</v-icon>
            </template>

        </v-data-table>
        <v-dialog v-model="dialogUpdate">
            <v-card>
                <v-form>
                    <v-container>
                        <v-row>
                            <v-col cols="12" md="6">
                                <v-text-field v-model="selectedItem && selectedItem.nombre" label="Nombre"></v-text-field>
                            </v-col>
                            <v-col cols="12" md="6">
                                <v-text-field v-model="selectedItem && selectedItem.apellido"
                                    label="Apellido"></v-text-field>
                            </v-col>
                        </v-row>
                        <v-row>
                            <v-col cols="12" md="6">
                                <v-select v-model="selectedItem && selectedItem.actividad" :items="actividades"
                                    label="Actividad"></v-select>
                            </v-col>
                            <v-col cols="12" md="6">
                                <v-select v-model="selectedItem && selectedItem.dia" :items="dias"
                                    :menu-props="{ maxHeight: '400' }" label="Dias" multiple hint="Selecciona tus dias"
                                    persistent-hint></v-select>

                            </v-col>
                        </v-row>
                        <v-row>
                            <v-col cols="12" md="6">
                                <v-slider v-model="selectedItem && selectedItem.hora" :min="0" :max="23" label="Hora"
                                    step="1" thumb-label></v-slider>
                            </v-col>
                            <v-col cols="12" md="6">
                                <v-radio-group v-model="selectedItem && selectedItem.opcion" row>
                                    <v-radio label="Efectivo" value="Efectivo"></v-radio>
                                    <v-radio label="Tarjeta" value="Tarjeta"></v-radio>
                                    <v-radio label="Bizum" value="Bizum"></v-radio>
                                </v-radio-group>
                            </v-col>
                        </v-row>
                        <v-btn @click="confirmUpdate(selectedItem)" color="primary">Enviar</v-btn>
                    </v-container>
                </v-form>
            </v-card>
        </v-dialog>

        <v-dialog v-model="dialogCreate">
            <pruebaForm  @pruebaCreate = pruebaCreate />

        </v-dialog>

    </v-card>
</template>


<script>


import pruebaRepository from '@/repository/PruebaRepository.js';
import pruebaForm from '@/components/pruebas/PruebasFrom.vue'


export default {
    data() {
        return {
            selectedItem: null,
            dialogUpdate: false,
            dialogCreate: false,
            search: '',
            headers: [
                {
                    text: 'Nombre',
                    align: 'start',
                    filterable: false,
                    value: 'nombre',
                },
                { text: 'Apelllidos', value: 'apellido' },
                { text: 'Actividad', value: 'actividad' },
                { text: 'Dias', value: 'dia' },
                { text: 'Hora', value: 'hora' },
                { text: 'Forma de Pago', value: 'opcion' },
                { text: 'Acciones', value: 'actions' }
            ],
            pruebas: [
            ],
            actividades: ['Fútbol', 'Natación', 'Tenis', 'Baloncesto'],
            dias: ['Lunes', 'Martes', 'Miércoles', 'Jueves', 'Viernes']

        }
    },
    created() {

        this.getPruebas()

    },

    components: {

        pruebaForm
    },

    methods: {

        async getPruebas() {
            try {

                this.pruebas = await pruebaRepository.getAll()
            }

            catch (error) {
                console.error(error)
            }
        },

        async deletePrueba(item) {

            try {
                await pruebaRepository.delete(item)
                this.getPruebas()
            }

            catch (error) {
                console.error(error)
            }

        },

        confirmDelete(item) {
            if (confirm("Estas seguro de eliminar")) {
                this.deletePrueba(item)
            }
        },

        async updatePrueba(item) {


            try {
                await pruebaRepository.upDate(item)
                this.getPruebas()
            }

            catch (error) {
                console.error(error)
            }

        },

        confirmUpdate(item) {
            if (confirm("Estas seguro de actualizar")) {
                this.updatePrueba(item)
                this.dialogUpdate = false
            }
        },
        openUpdateDialog(item) {
            this.selectedItem = { ...item }
            this.dialogUpdate = true
        },
        pruebaCreate(){
            this.dialogCreate = false
            this.getPruebas
        }
    }
}
</script>