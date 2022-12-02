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
          <Report />
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

      total: 0,
    }
  },
}
</script>
<style lang="scss"></style>
