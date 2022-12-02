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
          <tr v-for="item in detail" :key="item.id">
            <td class="text-center">{{ item.date }}</td>
            <td class="text-center">{{ item.type }}</td>
            <td class="text-center">{{ item.price }}</td>
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
          date: '2021-05-01',
          type: 'ห้องเล็ก',
          price: '1000',
        },
        {
          date: '2021-05-01',
          type: 'ห้องใหญ่',
          price: '3000',
        },
      ],
      total: 2000,
    }
  },
}
</script>
<style lang="scss"></style>
