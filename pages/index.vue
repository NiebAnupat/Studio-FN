<template>
  <div>
    <div class="d-flex justify-center mt-8">
      <v-card class="d-flex justify-center">
        <img src="~/assets/BG.png" height="600" width="1200" />
      </v-card>
    </div>
    <div class="d-flex justify-center mt-8">
      <v-btn to="/Cometo/" dark>เข้าสู่การจองห้อง</v-btn>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ store }) {
    store.dispatch('Auth/setAuthFalse')
  },
  name: 'index',
}
</script>
<style lang="scss"></style>
