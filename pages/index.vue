<template>
  <div>
    <v-row class="mt-8">
      <!-- Images -->
      <v-col cols="4">
        <v-card class="rounded-lg"
          ><v-carousel :show-arrows="false" height="380">
            <v-carousel-item
              v-for="(item, i) in items"
              :key="i"
              :src="item.src"
            ></v-carousel-item> </v-carousel
        ></v-card>
        <p class="d-flex justify-center mt-8">
          ยกเลิกการจอง ติดต่อ 091-234-5678
        </p>
      </v-col>

      <!-- Datail -->
      <v-col cols="8">
        <v-card height="600" class="pa-6 rounded-lg">
          <v-card-title> จองห้อง </v-card-title>
          <v-card-text>
            <v-row>
              <v-col cols="6"
                ><v-text-field name="name" label="ชื่อผู้จอง"></v-text-field
              ></v-col>
              <v-col cols="6"
                ><v-text-field name="tel" label="เบอร์ติดต่อ"></v-text-field
              ></v-col>
            </v-row>

            <v-row>
              <v-col cols="6"
                ><v-menu
                  v-model="datePicker"
                  :close-on-content-click="false"
                  :nudge-right="40"
                  transition="scale-transition"
                  offset-y
                  min-width="290"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-model="date"
                      label="วัน"
                      prepend-icon="mdi-calendar"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                    ></v-text-field>
                  </template>
                  <v-date-picker
                    v-model="date"
                    @input="datePicker = false"
                    scrollable
                    color="primary"
                  ></v-date-picker> </v-menu
              ></v-col>
            </v-row>

            <v-row>
              <v-col cols="">
                <v-menu
                  v-model="StarttimPicker"
                  :close-on-content-click="false"
                  :nudge-right="40"
                  transition="scale-transition"
                  offset-y
                  min-width="290"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-model="Starttime"
                      label="เวลาเริ่ม"
                      prepend-icon="mdi-alarm"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                    ></v-text-field>
                  </template>
                  <v-time-picker
                    v-model="Starttime"
                    @input="StarttimePicker = false"
                    scrollable
                    color="primary"
                  ></v-time-picker>
                </v-menu>
              </v-col>
              <v-col cols="6"
                ><v-menu
                  v-model="EndtimPicker"
                  :close-on-content-click="false"
                  :nudge-right="40"
                  transition="scale-transition"
                  offset-y
                  min-width="290"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-model="Endtime"
                      label="เวลาสิ้นสุด"
                      prepend-icon="mdi-alarm"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                    ></v-text-field>
                  </template>
                  <v-time-picker
                    v-model="Endtime"
                    @input="EndtimePicker = false"
                    scrollable
                    color="primary"
                  ></v-time-picker> </v-menu
              ></v-col>
            </v-row>

            <v-row>
              <v-col cols="6">
                <v-text-field
                  v-model="total"
                  label="ราคารวม"
                  readonly
                ></v-text-field>
              </v-col>
            </v-row>

            <v-row class="mt-4">
              <v-col cols="8">
                <p class="red--text">*โปรดตรวจสอบรายละเอียดก่อนกดตกลง</p>
              </v-col>
            </v-row>
          </v-card-text>

          <!-- Button -->
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" text nuxt @click="dialog = true">
              จอง
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>

    <div>
      <v-dialog v-model="dialog" scrollable :overlay="false" max-width="500px">
        <v-card height="560" width="200" class="pa-3 rounded-xl">
          <v-card-title> หลักฐานการโอนเงิน </v-card-title>
          <v-card-text class="d-flex justify-center mt-4">
            <v-row>
              <v-col cols="12" class="d-flex justify-center"
                ><img src="~/assets/Pic02.jpg" height="300"
              /></v-col>

              <v-col cols="12"
                ><v-file-input
                  v-model="editImg"
                  label="รูปภาพ"
                  show-size
                  counter
                  multiple
                  small
                  small-chips
                  ref="imgRef"
                  :rules="[(v) => !!v || 'กรุณาเลือกรูปภาพ']"
                ></v-file-input
              ></v-col>
            </v-row>
          </v-card-text>
          <v-card-actions>
            <v-btn color="primary" text nuxt @click="dialog = false">
              ยกเลิก
            </v-btn>
            <v-spacer></v-spacer>
            <v-btn color="primary" text nuxt @click="dialog = false">
              ยืนยัน
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Index',
  data() {
    return {
      dialog: false,
      date: null,
      datePicker: false,
      Starttime: null,
      StarttimePicker: false,
      Endtime: null,
      EndtimePicker: false,
      total: 0,
      items: [
        {
          src: 'https://cdn.vuetifyjs.com/images/carousel/squirrel.jpg',
        },
        {
          src: 'https://cdn.vuetifyjs.com/images/carousel/sky.jpg',
        },
        {
          src: 'https://cdn.vuetifyjs.com/images/carousel/bird.jpg',
        },
        {
          src: 'https://cdn.vuetifyjs.com/images/carousel/planet.jpg',
        },
      ],
    }
  },
}
</script>
<style lang="scss"></style>
