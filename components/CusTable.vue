<template>
  <div>
    <v-simple-table fixed-header height="350px">
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-center">ชื่อลูกค้า</th>
            <th class="text-center">เบอร์โทรศัพท์</th>
            <th class="text-center">วันที่</th>
            <th class="text-center">เวลาเริ่ม</th>
            <th class="text-center">เวลาสิ้นสุด</th>
            <th class="text-center"></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in detail" :key="item.id">
            <td class="text-center">{{ item.name }}</td>
            <td class="text-center">{{ item.tel }}</td>
            <td class="text-center">{{ item.date }}</td>
            <td class="text-center">{{ item.start }}</td>
            <td class="text-center">{{ item.end }}</td>
            <td class="text-center pa-2">
              <v-tooltip top>
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                    icon
                    v-bind="attrs"
                    v-on="on"
                    nuxt
                    @click="dialog = true"
                  >
                    <v-icon color="grey" class="mx-1"> mdi-book-search </v-icon>
                  </v-btn>
                </template>
                <span>สลิป</span>
              </v-tooltip>

              <v-tooltip top>
                <template v-slot:activator="{ on, attrs }">
                  <v-btn icon v-bind="attrs" v-on="on">
                    <v-icon color="red" class="mx-1"> mdi-minus </v-icon>
                  </v-btn>
                </template>
                <span>ยกเลิกการจอง</span>
              </v-tooltip>
            </td>
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
export default {
  data() {
    return {
      dialog: false,
      detail: [
        {
          name: 'นาย ศราวุธ พันธ์ดี',
          tel: '0812345678',
          date: '2020-01-01',
          start: '08:00',
          end: '17:00',
        },
        {
          name: 'นาย ชัยวัฒน์ แสงศรี',
          tel: '0912345678',
          date: '2020-01-02',
          start: '10.30',
          end: '19.00',
        },
      ],
      total: 2000,
    }
  },
}
</script>
<style lang="scss"></style>
