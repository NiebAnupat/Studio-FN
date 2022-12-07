<template>
  <div>
    <v-simple-table fixed-header height="350px">
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-center">วันที่</th>
            <th class="text-center">ประเภทห้อง</th>
            <th class="text-center">ราคา</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in rents" :key="item.RN_ID">
            <td class="text-center">{{ formarDate(item.RN_DATE) }}</td>
            <td class="text-center">{{ getTypeName(item.using_room[0].room.R_TYPE) }}</td>
            <td class="text-center">{{ item.RN_PRICE }}</td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>

    <div>
      <v-dialog v-model="dialog" scrollable :overlay="false" max-width="400px">
        <v-card height="500" class="pa-3 rounded-xl">
          <v-card-title> หลักฐานโอนเงิน </v-card-title>
          <v-card-text class="d-flex justify-center mt-4">
            <v-row class="d-flex justify-center"
              ><img src="~/assets/Pic03.jpg" height="300" />

              <v-text-field
                v-model="total"
                label="ราคารวม"
                readonly
              ></v-text-field
            ></v-row>
          </v-card-text>
        </v-card>
      </v-dialog>
    </div>
  </div>
</template>

<script>
import moment from "moment";
export default {
  props: {
    rents: {
      type: Array,
      // asyncData: true,
    },
  },
  data() {
    return {
      dialog: false,
      total: 2000,
    }
  },
  methods: {
    getTypeName(typeID){
      switch (typeID) {
        case 'sm':
          return 'ห้องเล็ก'
        case 'md':
          return 'ห้องกลาง'
        case 'lg':
          return 'ห้องใหญ่'
        default:
          return 'ไม่ระบุ'
      }
    },
    formarDate(date) {
      return moment(date).format("DD/MM/YYYY");
    },
  },
}
</script>
<style lang="scss"></style>
