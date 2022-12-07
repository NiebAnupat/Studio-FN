<template>
  <div class="mt-6">
    <div class="d-flex">
      <v-row>
        <v-col cols="4"><h1>สรุปรายได้ประจำเดือน</h1></v-col>
        <v-col cols="2"
          ><v-container>
            <v-select
              :items="month"
              label="เดือน"
              dense
              v-model="selected"
            ></v-select> </v-container
        ></v-col>
        <v-spacer></v-spacer>
        <v-spacer></v-spacer>
        <v-spacer></v-spacer>
        <v-spacer></v-spacer><v-spacer></v-spacer>
        <col />
      </v-row>
    </div>

    <div class="mt-2">
      <v-card class="rounded-xl pa-6" height="500">
        <div>
          <Report :rents="rents" />
        </div>
      </v-card>
    </div>

    <div class="mt-8 d-flex">
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>

      <v-text-field
        v-model="total"
        label="รวมทั้งหมด"
        class="mx-16"
      ></v-text-field>
    </div>
  </div>
</template>

<script>
import Report from '~/components/ReportTable.vue'
export default {
  name: 'report',
  components: {
    Report,
  },

  async asyncData({ store, $axios }) {
    store.dispatch('Auth/setAdminTrue')
    store.dispatch( 'Auth/setAuthTrue' )
    var rents = await $axios.$get('/rent/all')
    rents = rents.data
    var total = 0
    rents.forEach((element) => {
      total += element.RN_PRICE
    })
    return {
      rents,total
    }
  },

  data() {
    return {
      month: [
        'มกราคม',
        'กุมภาพันธ์',
        'มีนาคม',
        'เมษายน',
        'พฤษภาคม',
        'มิถุนายน',
        'กรกฎาคม',
        'สิงหาคม',
        'กันยายน',
        'ตุลาคม',
        'พฤศจิกายน',
        'ธันวาคม',
      ],
      selected: '',
      nMonth : '01',
    }
  },
  watch: {
    selected(){
      this.nMonth = this.month.indexOf(this.selected) + 1
      // nMonth to String 00
      if(this.nMonth < 10){
        this.nMonth = '0' + this.nMonth
      }
      console.log('/report/by/' + this.nMonth)
      this.$axios.$get('/report/by/' + this.nMonth).then((res) => {
        if ( !res ) {
          this.total = 0
          this.rents = []
        } else {
          // foreach
          res.forEach((element) => {
            this.total += element.RN_PRICE
          })
          this.rents = res
        }
      })
    }
  }
}
</script>
<style lang="scss"></style>
