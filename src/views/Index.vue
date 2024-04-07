<template>
    <v-app id="inspire">
        <v-app-bar app color="#395384" flat>
            <v-container class="py-0 fill-height">
                <!-- Sidebar para moviles ( < = 768 px de ancho)(Solo contiene el boton) -->
                <div class="movilSidebar movilSidebarBtn">
                    <v-btn dark @click.stop="drawer = !drawer">
                        <v-icon dark>mdi mdi-menu</v-icon>
                    </v-btn>
                </div>
                <v-spacer></v-spacer>
                <div class="normalHeaderbar">

                </div>
                <div class="movilSidebar movilSidebarBtn">
                    <v-menu offset-y>
                        <template v-slot:activator="{ on, attrs }">
                            <v-btn class="mx-2" fab dark small color="black" v-bind="attrs" v-on="on">
                                <v-icon dark>mdi mdi-menu</v-icon>
                            </v-btn>
                        </template>
                        <v-list>


                        </v-list>
                    </v-menu>
                </div>
                <div class="UserOptions">
                    <v-menu offset-y block>
                        <template v-slot:activator="{ on, attrs }">
                            <v-btn class="mx-2" fab dark small color="black" v-bind="attrs" v-on="on">
                                <v-icon dark>mdi mdi-account</v-icon>
                            </v-btn>
                        </template>
                        <v-list>
                            <v-row align="center">
                                <v-list-item>
                                    <v-col cols="12" class="text-center">
                                        <v-list-item-avatar size="100">
                                            <v-img src="https://cdn.vuetifyjs.com/images/john.png"></v-img>
                                        </v-list-item-avatar>
                                        <v-list-item-content>
                                            <v-list-item-title class="text-h4">
                                                ¡Hola, Sebastian!
                                            </v-list-item-title>
                                            <v-list-item-subtitle>john@vuetifyjs.com</v-list-item-subtitle>
                                        </v-list-item-content>
                                    </v-col>
                                </v-list-item>
                            </v-row>
                        </v-list>
                        <v-divider></v-divider>
                        <!-- <v-list>
                            <v-btn text @click="logout" block>
                                <v-list-item class="d-flex justify-center">
                                    <v-icon>mdi-logout</v-icon>
                                    <span class="text-h6" style="margin-left: 12px;">Salir</span>
                                </v-list-item>
                            </v-btn>
                        </v-list> -->
                    </v-menu>
                </div>
            </v-container>
        </v-app-bar>
        <v-navigation-drawer v-model="drawer" absolute temporary class="movilSidebar">
            <v-list class="rounded-lg" nav shaped>
                <v-list nav shaped>
                    <v-list-item v-for="(Icatego, IndeCatindex) in IndependentCategories" :key="IndeCatindex"
                        @click="selectedComponent = Icatego.component" link>
                        <v-list-item-action>
                            <v-icon>{{ Icatego.icon }}</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title>{{ Icatego.title }}</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                </v-list>

            </v-list>
        </v-navigation-drawer>
        <v-main class="grey lighten-3" style="padding: 20px 20px 20px;">
            <v-row>
                <div class="normalSidebar ">
                    <v-col>
                        <v-sheet class="rounded-lg">
                            <v-list class="rounded-lg" nav shaped>
                                <v-list nav shaped>
                                    <v-list-item v-for="(Icatego, IndeCatindex) in IndependentCategories"
                                        :key="IndeCatindex" @click="selectedComponent = Icatego.component" link>
                                        <v-list-item-action>
                                            <v-icon>{{ Icatego.icon }}</v-icon>
                                        </v-list-item-action>
                                        <v-list-item-content>
                                            <v-list-item-title>{{ Icatego.title }}</v-list-item-title>
                                        </v-list-item-content>
                                    </v-list-item>
                                </v-list>

                            </v-list>
                        </v-sheet>
                    </v-col>
                </div>



                <v-col>
                    <v-sheet min-height="75vh" rounded="lg" style="width: 100%; height: auto;">
                        <component v-if="selectedComponent" :is="selectedComponent"></component>
                    </v-sheet>
                </v-col>
            </v-row>

        </v-main>
    </v-app>
</template>

<script>

import Employees from '@/components/Employees.vue';
import Reservation from '@/components/Reservation.vue';




export default {
    data: () => ({
        components: {
            Employees,
            // Clientes,
            // Contratos,
            // Empleados,
        },
        drawer: null,
        rail: true,
        railHeader: true,
        loading: true,
        selectedGroup: null,
        selectedComponent: Reservation,
        links: ['Dashboard', 'Messages', 'Profile', 'Updates'],
        // menuItems: [
        //     { title: 'Inicio', link: "Contratos" },
        //     { title: 'Productos', link: '/productos' },
        //     { title: 'Servicios', link: '/servicios' },
        //     // Agrega más elementos del menú aquí
        // ],
        perfilItems: [
            { title: 'Perfil', icon: 'mdi-account-circle', link: 'Contratos' },
            { title: 'Configuracion', icon: 'mdi-cog', link: '' },
            { title: 'Acerca de', icon: 'mdi-information', link: '' },
            { title: 'Salir', icon: 'mdi-logout', click: 'logout' }
        ],
        IndependentCategories: [
            { title: 'Inicio', component: Reservation, icon: 'mdi-home' },
            { title: 'Calendario', component: Employees, icon: 'mdi-calendar' },
        ],
    }),


}
</script>

<style>
/* Estilos para pantallas mayores o iguales a 768px (normalSideBar visible, movilBar oculto) */
@media screen and (min-width: 767px) {





    .movilBar,
    .movilSidebar {
        display: none;
        /* Ocultar la clase movilBar */
    }
}

/* Estilos para pantallas menores a 768px (normalSideBar oculto, movilBar visible) */
@media screen and (max-width: 768px) {

    .normalSidebar,
    .normalHeaderbar {
        display: none;
        /* Ocultar la clase normalSideBar */
    }


}
</style>