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
                                        <v-btn @click="deleteData(props.item.id)">
                                            <v-icon>mdi-delete</v-icon>
                                        </v-btn>
                                        <v-btn @click="editData(props.item.id)">
                                            <v-icon>mdi-note-edit</v-icon>
                                        </v-btn>
                                    </v-btn-toggle>
                                </td>
                            </tr>
                        </template>
                    </v-data-table>
                </v-card>
            </v-col>


        </v-row>


        <v-btn color="primary" @click="showModal = true">Abrir Formulario</v-btn>

        <v-dialog v-model="showModal" max-width="1200">
            <v-card>
                <v-card-title>
                    <span>Crear reserva </span>
                </v-card-title>

                <v-card-text>
                    <v-form @submit.prevent="submitForm">
                        <!-- Sección 1 -->
                        <v-divider></v-divider>
                        <span>Informacion de la reserva </span>

                        <br />
                        <v-row>
                            <v-col cols="12" md="4">
                                <v-text-field v-model="Reservation.service" label="Servicio" required outlined
                                    :counter="50" maxlength="50"></v-text-field>
                            </v-col>
                            <v-col cols="12" md="4">
                                <v-menu ref="menu" v-model="menuTime" :close-on-content-click="false" :nudge-right="40"
                                    :return-value.sync="Reservation.hour" transition="scale-transition" offset-y
                                    max-width="290px" min-width="290px">
                                    <template v-slot:activator="{ on, attrs }">
                                        <v-text-field v-model="Reservation.hour" label="Horario"
                                            prepend-icon="mdi-clock-time-four-outline" readonly v-bind="attrs"
                                            v-on="on"></v-text-field>
                                    </template>
                                    <v-time-picker v-if="menuTime" v-model="Reservation.hour" full-width
                                        @click:minute="$refs.menu.save(Reservation.hour)"></v-time-picker>
                                </v-menu>
                            </v-col>

                            <v-col cols="12" md="4">
                                <v-menu v-model="menu2" :close-on-content-click="false" :nudge-right="40"
                                    transition="scale-transition" offset-y min-width="auto">
                                    <template v-slot:activator="{ on, attrs }">
                                        <v-text-field v-model="Reservation.date" label="Picker without buttons"
                                            prepend-icon="mdi-calendar" readonly v-bind="attrs"
                                            v-on="on"></v-text-field>
                                    </template>
                                    <v-date-picker v-model="Reservation.date" @input="menu2 = false"></v-date-picker>
                                </v-menu>
                            </v-col>
                            <v-col cols="12" md="4">
                                <v-text-field v-model="Reservation.pick_up" label="Pick Up" required outlined
                                    :counter="50" maxlength="50"></v-text-field>
                            </v-col>
                            <v-col cols="12" md="4">
                                <v-text-field v-model="Reservation.destination" label="Destino" required outlined
                                    prepend-inner-icon="mdi-map-marker" :counter="50" maxlength="50"></v-text-field>
                            </v-col>
                            <v-col cols="12" md="4">
                                <v-text-field v-model="Reservation.cost" label="Costo" required outlined :counter="5"
                                    prepend-inner-icon="mdi-currency-usd" maxlength="5"
                                    :rules="[numero => /^[0-9]*$/.test(numero) || 'Ingrese solo números']"></v-text-field>
                            </v-col>
                        </v-row>
                        <br />
                        <v-divider></v-divider>
                        <span>Informacion del cliente </span>

                        <br />

                        <!-- Sección 2 -->
                        <v-row>
                            <v-col cols="12" md="4">
                                <v-text-field v-model="Reservation.client_name" label="Nombre del cliente" required
                                    outlined :counter="50" maxlength="50"></v-text-field>
                            </v-col>
                            <v-col cols="12" md="4">
                                <v-text-field v-model="Reservation.phone" label="Telefono del cliente" required outlined
                                    :rules="[numero => /^[0-9]*$/.test(numero) || 'Ingrese solo números']" :counter="10"
                                    maxlength="10"></v-text-field>
                            </v-col>
                            <v-col cols="12" md="4">
                                <v-text-field v-model="Reservation.passengers" label="Numero de pasajeros" required
                                    :rules="[numero => /^[0-9]*$/.test(numero) || 'Ingrese solo números']" outlined
                                    :counter="2" maxlength="2"></v-text-field>
                            </v-col>
                        </v-row>
                        <br />
                        <v-divider></v-divider>
                        <span>Informacion del vendedor </span>

                        <br />

                        <v-row>
                            <v-col cols="12" md="6">
                                <v-text-field v-model="Reservation.seller" label="Vendedor" required outlined
                                    :counter="50" maxlength="50"></v-text-field>
                            </v-col>
                            <v-col cols="12" md="6">
                                <v-text-field v-model="Reservation.seller_charge" label="Comicion del vendedor" required
                                    prepend-inner-icon="mdi-currency-usd" outlined :counter="5"
                                    :rules="[numero => /^[0-9]*$/.test(numero) || 'Ingrese solo números']"
                                    maxlength="5"></v-text-field>
                            </v-col>
                        </v-row>
                        <br />

                        <v-divider></v-divider>
                        <span>Informacion del conductor </span>

                        <br />
                        <v-row>
                            <v-col cols="12" md="4">
                                <v-text-field v-model="Reservation.driver" label="Conductor" required outlined
                                    :counter="50" maxlength="50"></v-text-field>
                            </v-col>
                            <v-col cols="12" md="4">
                                <v-text-field v-model="Reservation.driver_payment" label="Pago del conductor" required
                                    prepend-inner-icon="mdi-currency-usd" outlined :counter="5"
                                    :rules="[numero => /^[0-9]*$/.test(numero) || 'Ingrese solo números']"
                                    maxlength="5"></v-text-field>
                            </v-col>
                            <v-col cols="12" md="4">
                                <v-text-field v-model="Reservation.commission" label="Comision del conductor" required
                                    prepend-inner-icon="mdi-currency-usd" outlined :counter="6"
                                    :rules="[numero => /^[0-9]*$/.test(numero) || 'Ingrese solo números']"
                                    maxlength="6"></v-text-field>
                            </v-col>
                        </v-row>
                        <br />

                        <v-divider></v-divider>
                        <span>Informacion extra </span>

                        <br />
                        <!-- Sección 5 -->
                        <v-row>
                            <v-col cols="12" md="4">
                                <v-text-field v-model="Reservation.remainder" label="Remainder" required outlined
                                    :counter="50" maxlength="50"></v-text-field>
                            </v-col>

                            <v-col cols="12" md="4">
                                <v-text-field v-model="Reservation.extra_requirements" label="Extra Requirements"
                                    required outlined :counter="50" maxlength="50"></v-text-field>
                            </v-col>
                            <!-- Aquí puedes agregar más columnas si es necesario -->
                        </v-row>
                    </v-form>
                </v-card-text>

                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="showModal = false">Cancelar</v-btn>
                    <v-btn color="blue darken-1" @click="submitForm" :disabled="!isFormValid">Enviar</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>


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
            menu2: false,

            menu: false,
            modal: false,
            menuTime: false,
            showModal: false,
            menuDate: false,
            Reservation: {
                service: "",
                hour: "",
                date: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
                client_name: "",
                phone: "",
                passengers: "",
                pick_up: "",
                destination: "",
                cost: "",
                seller: "",
                seller_charge: "",
                commission: "",
                driver: "",
                driver_payment: "",
                remainder: "",
                extra_requirements: "",
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

    },
    computed: {
        isFormValid() {
            const {
                service,
                hour,
                date,
                client_name,
                phone,
                passengers,
                pick_up,
                destination,
                cost,
                seller,
                seller_charge,
                commission,
                driver,
                driver_payment,
                remainder,
                extra_requirements,
            } = this.Reservation;
            return (
                service &&
                hour &&
                date &&
                client_name &&
                phone &&
                passengers &&
                pick_up &&
                destination &&
                cost &&
                seller &&
                seller_charge &&
                commission &&
                driver &&
                driver_payment &&
                remainder &&
                extra_requirements
            );
        },
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
        },

        async deleteData(id) {
            console.log("ID:", id)
            fetch(`http://localhost:3000/Reservation/${id}`, {
                method: 'DELETE'
            })
                .then(response => {
                    if (!response.ok) {
                        throw new Error('No se pudo eliminar el dato');
                    } else {

                        console.log("ID:", id, "eliminado de manera correcta")
                        this.getData();
                        // Recargar la página para mostrar los cambios actualizados
                        window.location.reload();
                    }
                    // Actualizar la interfaz de usuario después de eliminar el dato
                    // Por ejemplo, eliminar el dato de una lista o recargar la lista de datos
                })
                .catch(error => {
                    console.error('Error al eliminar el dato:', error);
                });
        },


        editData(id) {

            console.log('ID:', id)
        },
        submitForm() {
            console.log("Info:", this.Reservation);
            fetch("http://localhost:3000/Reservation", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(this.Reservation),
            })
                .then((response) => {
                    if (!response.ok) {
                        throw new Error("Error al enviar los datos.");
                    } else {
                        console.log("Enviado correctamente")
                        window.location.reload();
                        this.getData();

                    }
                    return response.json();
                })
                .then((data) => {
                    console.log("Respuesta del servidor:", data);
                    this.showModal = false;
                })
                .catch((error) => {
                    console.error("Error al enviar los datos:", error);
                });
        },
    }


}
</script>