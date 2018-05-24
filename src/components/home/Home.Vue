<template>
    <v-app>
        <v-navigation-drawer fixed v-model="drawer" app>
            <v-list dense>
                <v-list-tile @click="">
                    <v-list-tile-action>
                        <v-icon>home</v-icon>
                    </v-list-tile-action>
                    <v-list-tile-content>
                        <v-list-tile-title>Inicio</v-list-tile-title>
                    </v-list-tile-content>
                </v-list-tile>
                <v-list-tile @click="">
                    <v-list-tile-action>
                        <v-icon>contact_mail</v-icon>
                    </v-list-tile-action>
                    <v-list-tile-content>
                        <v-list-tile-title>Contacto</v-list-tile-title>
                    </v-list-tile-content>
                </v-list-tile>
            </v-list>
        </v-navigation-drawer>
        <v-toolbar color="purple" dark fixed app>
            <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
            <v-toolbar-title>VueCine</v-toolbar-title>
        </v-toolbar>
        <v-content>
            <app-header></app-header>
        </v-content>
        <!-- Footer -->
        <v-footer dark app>
            <v-container grid-list-xs class="text-xs-center">
                <span class="white--text">VueCine &copy; 2018</span>
            </v-container>
        </v-footer>
    </v-app>
</template>

<script>
import Header from './Header.vue';
export default {
    data: () => ({
        drawer: false
    }),
    props: {
        source: String
    },
    components: {
        appHeader: Header
    }
};
</script>
