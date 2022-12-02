<template>
  <div class="mt-6">
    <h1>ข้อมูลลูกค้า</h1>

    <div class="mt-8">
      <v-card class="rounded-xl pa-6" height="550">
        <div>
          <CusTable />
        </div>
      </v-card>
    </div>
  </div>
</template>

<script>
import CusTable from '~/components/CusTable.vue'
export default {
  name: 'detail',
  async aysncData({ store }) {
    store.dispatch('Auth/setAdminTrue')
  },

  components: {
    CusTable,
  },
}
</script>
<style lang="scss"></style>
