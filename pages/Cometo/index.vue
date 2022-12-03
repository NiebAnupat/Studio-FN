<template>
  <div>
    <v-row class="mt-8">
      <!-- Images -->
      <v-col cols="4">
        <v-card class="rounded-lg">
          <v-carousel :show-arrows="false" height="600" cycle>
            <v-carousel-item
              v-for="(image, i) in rImage"
              :key="i"
              :src="image"
            ></v-carousel-item>
          </v-carousel>
        </v-card>
        <p class="d-flex justify-center mt-8">
          ยกเลิกการจอง ติดต่อ 091-234-5678
        </p>
      </v-col>

      <!-- Datail -->
      <v-col cols="8">
        <v-card height="600" class="pa-4 rounded-lg">
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
                  v-model="startTimePicker"
                  :close-on-content-click="false"
                  :nudge-right="40"
                  transition="scale-transition"
                  offset-y
                  min-width="290"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-model="startTime"
                      label="เวลาเริ่ม"
                      prepend-icon="mdi-alarm"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                    ></v-text-field>
                  </template>
                  <v-time-picker
                    v-model="startTime"
                    @input="startTimePicker = false"
                    scrollable
                    color="primary"
                  ></v-time-picker>
                </v-menu>
              </v-col>
              <v-col cols="6"
                ><v-menu
                  v-model="endTimePicker"
                  :close-on-content-click="false"
                  :nudge-right="40"
                  transition="scale-transition"
                  offset-y
                  min-width="290"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-model="endTime"
                      label="เวลาสิ้นสุด"
                      prepend-icon="mdi-alarm"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                    ></v-text-field>
                  </template>
                  <v-time-picker
                    v-model="endTime"
                    @input="endTimePicker = false"
                    scrollable
                    color="primary"
                  ></v-time-picker> </v-menu
              ></v-col>
            </v-row>

            <v-row>
              <v-col cols="6">
                <v-container fluid>
                  <v-select :items="type" label="ประเภทห้อง" dense></v-select>
                </v-container>
              </v-col>
              <v-col cols="6">
                <v-text-field
                  v-model="price"
                  label="ราคาห้อง"
                  readonly
                ></v-text-field>
              </v-col>
            </v-row>

            <v-row>
              <v-col cols="6">
                <v-text-field label="ราคารวม" readonly></v-text-field>
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

    <!-- Dialog -->
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
                  v-model="file"
                  label="รูปภาพ"
                  show-size
                  counter
                  multiple
                  small
                  small-chips
                  ref="imgRef"
                ></v-file-input
              ></v-col>
            </v-row>
          </v-card-text>
          <v-card-actions>
            <v-btn color="primary" text nuxt @click="dialog = false">
              ยกเลิก
            </v-btn>
            <v-spacer></v-spacer>
            <v-btn
              color="primary"
              text
              nuxt
              @click=";(dialog = false), (snackbar = true)"
            >
              ยืนยัน
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>

      <div>
        <v-snackbar v-model="snackbar" :timeout="2000">
          จองห้องสำเร็จ
        </v-snackbar>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'index',
  async asyncData({ store }) {
    store.dispatch('Auth/setAuthTrue')
  },

  data() {
    return {
      dialog: false,
      snackbar: false,
      date: null,
      startTime: null,
      endTime: null,
      datePicker: false,
      startTimePicker: false,
      endTimePicker: false,
      file: null,
      price: 0,
      type: ['ห้องเล็ก', 'ห้องกลาง', 'ห้องใหญ่'],

      rImage: [
        require('~/assets/show01.png'),
        require('~/assets/show02.png'),
        require('~/assets/show03.png'),
        require('~/assets/show04.png'),
        require('~/assets/show05.png'),
        require('~/assets/show06.png'),
      ],
    }
  },

  methods: {
    toImgUrl(img) {
      return URL.createObjectURL(img)
    },
  },
}
</script>
<style lang="scss"></style>
