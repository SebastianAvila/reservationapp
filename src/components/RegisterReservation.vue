<template>
    <v-container>
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
                                <v-menu v-model="menuDate" :close-on-content-click="false" :nudge-right="40"
                                    transition="scale-transition" offset-y min-width="auto">
                                    <template v-slot:activator="{ on, attrs }">
                                        <v-text-field v-model="date" label="Fecha" prepend-icon="mdi-calendar" readonly
                                            v-bind="attrs" v-on="on"></v-text-field>
                                    </template>
                                    <v-date-picker v-model="Reservation.date" required outlined
                                        @input="menuDate = false"></v-date-picker>
                                </v-menu>
                            </v-col>

                            <v-col cols="12" md="4">
                                <v-text-field v-model="Reservation.pick_up" label="Pick Up" required outlined
                                    :counter="50" maxlength="50"></v-text-field>
                            </v-col>
                            <v-col cols="12" md="4">
                                <v-text-field v-model="Reservation.destination" label="Destino" required outlined
                                    :counter="50" maxlength="50"></v-text-field>
                            </v-col>
                            <v-col cols="12" md="4">
                                <v-text-field v-model="Reservation.cost" label="Costo" required outlined :counter="50"
                                    maxlength="50"></v-text-field>
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
                                    :counter="50" maxlength="50"></v-text-field>
                            </v-col>
                            <v-col cols="12" md="4">
                                <v-text-field v-model="Reservation.passengers" label="Numero de pasajeros" required
                                    outlined :counter="50" maxlength="50"></v-text-field>
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
                                    outlined :counter="50" maxlength="50"></v-text-field>
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
                                    outlined :counter="50" maxlength="50"></v-text-field>
                            </v-col>
                            <v-col cols="12" md="4">
                                <v-text-field v-model="Reservation.commission" label="Comision del conductor" required
                                    outlined :counter="50" maxlength="50"></v-text-field>
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
export default {
    data() {
        return {
            date: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
                .toISOString()
                .substr(0, 10),
            menu: false,
            modal: false,
            menuTime: false,
            showModal: false,
            menuDate: false,
            Reservation: {
                service: "",
                hour: "",
                date: "",
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
        };
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
    },
};
</script>