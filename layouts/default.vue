<template>
  <v-app>
    <div>
      <v-app-bar dark v-if="isAuth">
        <v-btn icon class="mx-2"@click="goHome"
          ><v-icon>mdi-guitar-electric</v-icon></v-btn
        >
        <v-toolbar-title class="mx-n4">i - Zank Studio's</v-toolbar-title>

        <div v-if="isAdmin" class="mx-12">
          <v-btn text to="/Cometo/Admin/Detail">หน้าหลัก</v-btn>
          <v-btn text to="/Cometo/Admin/Report" class="mx-2">รายงาน</v-btn>
        </div>

        <v-spacer></v-spacer>

        <!-- Login / Logout -->
        <v-menu offset-y>
          <template v-slot:activator="{ on, attrs }">
            <v-btn icon v-bind="attrs" v-on="on">
              <v-icon>mdi-account-circle-outline</v-icon>
            </v-btn>
          </template>
          <v-list>
            <v-list-item>
              <v-btn text @click="logout">Login</v-btn>
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
    }
  },

  methods: {
    async logout() {
      await this.$store.dispatch('Auth/setAdminFalse')
      this.$router.push('/Cometo/Admin')
    },

    goHome(){
      this.$store.dispatch('Auth/setAdminFalse')
      this.$router.push('/')
    }
  },

  computed: {
    isAdmin() {
      return this.$store.getters['Auth/isAdmin']
    },
    isAuth() {
      return this.$store.getters['Auth/isAuth']
    },
  },
}
</script>
