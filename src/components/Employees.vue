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
                    <v-data-table v-if="!loading" :headers="headers" :items="Employees" :items-per-page="5"
                        :footer-props="{
                            showFirstLastPage: true,
                            firstIcon: 'mdi-arrow-collapse-left',
                            lastIcon: 'mdi-arrow-collapse-right',
                            prevIcon: 'mdi-minus',
                            nextIcon: 'mdi-plus'
                        }">
                        <template v-slot:item="props">
                            <tr>
                                <td>{{ props.item.name }}</td>
                                <td>{{ props.item.age }}</td>
                                <td>{{ props.item.birth_date }}</td>
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


        </v-row>
    </v-container>
</template>
<script>
import Employees from './Employees.vue';

export default {
    data() {
        return {
            loading: true, // Agregamos la variable de carga
            headers: [
                {
                    text: 'Nombre',
                    align: 'center',
                    sortable: false,
                    value: 'name',
                },
                {
                    text: 'Edad',
                    align: 'center',
                    sortable: false,
                    value: 'ege'
                },
                {
                    text: 'Cumpleaños',
                    align: 'center',
                    sortable: false,
                    value: 'birth_date'
                },
                {
                    text: 'Ver Mas',
                    align: 'center',
                    sortable: false,
                    value: ''
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
    methods: {
        // Obtiene la data que se muestra al cargar la página 
        async getData() {
            const urlGet = "http://localhost:3000/Employees"
            try {
                const response = await fetch(urlGet);
                if (response.ok) {
                    const data = await response.json();
                    console.log("Response", data)
                    this.Employees = data.Employees;
                    console.log("Employees", this.Employees)
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