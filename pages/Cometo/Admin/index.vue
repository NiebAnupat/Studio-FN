<template>
  <div class="d-flex justify-center mt-16">
    <v-card height="300" width="400" class="rounded-xl pa-2">
      <v-card-title class="d-flex justify-center">
        เข้าสู่ระบบแอดมิน
      </v-card-title>
      <v-card-text
        ><v-row>
          <v-col col="12">
            <!-- Input Username -->
            <v-text-field
              prepend-inner-icon="mdi-email"
              v-model="id"
              label="ชื่อผู้ใช้"
              required
              @keydown.enter="login"
            ></v-text-field>

            <!-- Input Password -->
            <v-text-field
              prepend-inner-icon="mdi-lock"
              v-model="password"
              label="รหัสผ่าน"
              :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
              :type="show1 ? 'text' : 'password'"
              @click:append="show1 = !show1"
              @keydown.enter="login"
            ></v-text-field>
          </v-col> </v-row
      ></v-card-text>
      <v-card-actions>
        <v-btn color="primary" text nuxt to="/Cometo"> กลับ </v-btn>
        <v-spacer></v-spacer>
        <v-btn color="primary" text nuxt @click="login"> เข้าสู่ระบบ </v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script>
export default {
  name: 'index',

  data() {
    return {
      id: '',
      password: '',
      show1: false,
    }
  },

  methods: {
    async login() {
      if (this.id == 'admin' && this.password == 'admin') {
        await this.$store.dispatch('Auth/setAdminTrue')
        this.$router.push('./admin/Detail')
      } else {
        this.$store.dispatch('Auth/setAdminFalse')
        this.$swal({
          title: 'เข้าสู่ระบบไม่สำเร็จ',
          text: 'ชื่อผู้ใช้หรือรหัสผ่านไม่ถูกต้อง',
          type: 'error',
        })
      }
    },
  },
}
</script>
<style lang="scss"></style>
