<template>
  <v-app>
    <div>
      <v-app-bar dark>
        <v-icon>mdi-guitar-electric</v-icon>
        <v-toolbar-title class="mx-5">i - Zank Studio's</v-toolbar-title>

        <v-spacer></v-spacer>

        <v-menu offset-y>
          <template v-slot:activator="{ on, attrs }">
            <v-btn icon v-bind="attrs" v-on="on">
              <v-icon>mdi-account-circle-outline</v-icon>
            </v-btn>
          </template>
          <v-list>
            <v-list-item>
              <v-list-item-title
                ><v-btn text nuxt to="/admin">login</v-btn></v-list-item-title
              >
            </v-list-item>
          </v-list>
        </v-menu>
      </v-app-bar>
    </div>

    <v-container class="ma-auto" style="height: 95%">
      <nuxt class="ml-10 pb-5" />
    </v-container>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data() {
    return {
      dialog: false,
      show1: false,
      id: '',
      password: '',
    }
  },
}
</script>
