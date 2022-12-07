<template>
  <div class="mt-6">
    <h1>ข้อมูลลูกค้า</h1>

    <div class="mt-8">
      <v-card class="rounded-xl pa-6" height="550">
        <div>
          <CusTable :rents="rents.data" :refreshData="refreshData"/>
        </div>
      </v-card>
    </div>
  </div>
</template>

<script>
import CusTable from '~/components/CusTable.vue'

export default {
  name : 'detail',
  async asyncData( { store, $axios } ) {
    store.dispatch( 'Auth/setAdminTrue' )
    store.dispatch( 'Auth/setAuthTrue' )
    const rents = await $axios.$get( '/rent/all' )
    // console.log( rents.data )
    return {
      rents
    }
  },

  methods : {
    async refreshData() {
      const rents = await this.$axios.$get( '/rent/all' )
      console.log(rents)
      this.rents = rents
    }
  },


  components : {
    CusTable,
  },
}
</script>
<style lang="scss"></style>
