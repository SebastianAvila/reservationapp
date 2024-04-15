<template>
    <v-container>
        <v-row class="text-center">
            <v-col cols="12">
                <v-card>
                    <v-card-title>
                        Datos
                        <v-spacer></v-spacer>
                    </v-card-title>

                    <v-skeleton-loader v-if="loading" type="table"></v-skeleton-loader>
                    <!-- Skeleton Loader para la tabla -->
                    <!-- Mostrar la tabla cuando loading sea false -->
                    <v-data-table v-if="!loading" :headers="headers" :items="Reservations" :items-per-page="5"
                        :footer-props="{
                            showFirstLastPage: true,
                            firstIcon: 'mdi-arrow-collapse-left',
                            lastIcon: 'mdi-arrow-collapse-right',
                            prevIcon: 'mdi-minus',
                            nextIcon: 'mdi-plus'
                        }">
                        <template v-slot:item="props">
                            <tr>
                                <td>{{ props.item.hour }}</td>
                                <td>{{ props.item.pick_up }}</td>
                                <td>{{ props.item.destination }}</td>
                                <td>{{ props.item.cost }}</td>
                                <td>{{ props.item.seller }}</td>
                                <td>{{ props.item.seller_charge }}</td>
                                <td>{{ props.item.driver }}</td>
                                <td>{{ props.item.commission }}</td>
                                <td>{{ props.item.remainder }}</td>
                                <td>{{ props.item.extra_requirements }}</td>
                                <td>
                                    <v-btn-toggle borderless>
                                        <v-btn @click="getSecondData(props.item)">
                                            <v-icon>mdi-open-in-new</v-icon>
                                        </v-btn>
                                    </v-btn-toggle>
                                </td>
                            </tr>
                        </template>
                    </v-data-table>
                </v-card>
            </v-col>

            <RegisterReservation />
        </v-row>
    </v-container>

</template>


<script>

import RegisterReservation from './RegisterReservation.vue';
export default {
    data() {

        return {
            loading: true, // Agregamos la variable de carga
            headers: [
                {
                    text: 'Hora servicio',
                    align: 'center',
                    sortable: true,
                    value: 'hour',
                },
                {
                    text: 'PickUp',
                    align: 'center',
                    sortable: true,
                    value: 'pick_up',
                },
                {
                    text: 'Destino',
                    align: 'center',
                    sortable: true,
                    value: 'destination',
                },
                {
                    text: 'Costo',
                    align: 'center',
                    sortable: true,
                    value: 'cost',
                },
                {
                    text: 'Vendedor',
                    align: 'center',
                    sortable: true,
                    value: 'seller',
                },
                {
                    text: 'Comision vendedor',
                    align: 'center',
                    sortable: true,
                    value: 'seller_charge',
                },
                {
                    text: 'Conductor',
                    align: 'center',
                    sortable: true,
                    value: 'drive',
                },
                {
                    text: 'Comision conductor',
                    align: 'center',
                    sortable: true,
                    value: 'commission',
                },
                {
                    text: 'Remanente',
                    align: 'center',
                    sortable: true,
                    value: 'remainder',
                },
                {
                    text: 'Requisitos extras',
                    align: 'center',
                    sortable: true,
                    value: 'extra_requirements',
                },
                {
                    text: 'Acciones',
                    align: 'center',
                    sortable: false,

                },



            ],
            Records: [],
            responseData: {
                Code: "",
                CurrentPage: "",
                Message: "",
                Description: "",
                IdSatCode: "",
                IncludeIvaTransferred: "",
                IncludeIepsTransferred: "",
                ProdServCode: "",
            },
            dialog: false
        }
    },
    created() {
        //Carga la función
        this.getData();
    },
    components: {

        RegisterReservation
        // Clientes,
        // Contratos,
        // Empleados,
    },
    methods: {
        // Obtiene la data que se muestra al cargar la página 
        async getData() {
            const urlGet = "http://localhost:3000/Reservation"
            try {
                const response = await fetch(urlGet);
                if (response.ok) {
                    const data = await response.json();
                    console.log("Response", data)
                    this.Reservations = data.Reservations;
                    console.log("Reservations", this.Reservations)
                    this.loading = false; // Desactivamos la variable de carga
                } else {
                    console.error('Error al obtener datos:', response.statusText);
                }
            } catch (error) {
                console.error('Error al obtener datos:', error);
            }
        }
    }
}
</script>