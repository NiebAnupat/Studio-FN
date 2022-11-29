<template>
  <div class="mt-4">
    <h1>ข้อมูลลูกค้า</h1>
    <div class="mt-14">
      <v-card class="rounded-xl pa-6">
        <div>
          <CusTable />
        </div>
      </v-card>
    </div>
  </div>
</template>

<script>
import CusTable from '../../components/CusTable.vue'
export default {
  components: {
    CusTable,
  },

  data() {
    return {
      CusTable: true,
    }
  },
}
</script>
<style lang="scss"></style>
