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
          <v-card-title> จองห้อง</v-card-title>
          <v-card-text>
            <v-row>
              <v-col cols="6"
              >
                <v-text-field name="name" v-model="rent.CUS_NAME" label="ชื่อผู้จอง"></v-text-field
                >
              </v-col>
              <v-col cols="6"
              >
                <v-text-field name="tel" :disabled="!rent.CUS_NAME" v-model="rent.CUS_TEL" label="เบอร์ติดต่อ" :rules="telRules"></v-text-field
                >
              </v-col>
            </v-row>

            <v-row>
              <v-col cols="6"
              >
                <v-menu
                  v-model="datePicker"
                  :close-on-content-click="false"
                  :nudge-right="40"
                  transition="scale-transition"
                  offset-y
                  min-width="290"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-model="rent.RN_DATE"
                      :disabled="!rent.CUS_TEL"
                      label="วัน"
                      prepend-icon="mdi-calendar"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                    ></v-text-field>
                  </template>
                  <v-date-picker
                    v-model="rent.RN_DATE"
                    @input="datePicker = false"
                    scrollable
                    color="primary"
                  ></v-date-picker>
                </v-menu
                >
              </v-col>
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
                      :disabled="!rent.RN_DATE"
                      v-model="rent.RN_START_TIME"
                      label="เวลาเริ่ม"
                      prepend-icon="mdi-alarm"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                    ></v-text-field>
                  </template>
                  <v-time-picker
                    v-model="rent.RN_START_TIME"
                    @input="startTimePicker = false"
                    scrollable
                    format="24hr"
                    color="primary"
                  ></v-time-picker>
                </v-menu>
              </v-col>
              <v-col cols="6"
              >
                <v-menu
                  v-model="endTimePicker"
                  :close-on-content-click="false"
                  :nudge-right="40"
                  transition="scale-transition"
                  offset-y
                  min-width="290"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-model="rent.RN_END_TIME"
                      :disabled="!rent.RN_START_TIME"
                      label="เวลาสิ้นสุด"
                      prepend-icon="mdi-alarm"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                    ></v-text-field>
                  </template>
                  <v-time-picker
                    v-model="rent.RN_END_TIME"
                    @input="endTimePicker = false"
                    scrollable
                    format="24hr"
                    color="primary"
                  ></v-time-picker>
                </v-menu
                >
              </v-col>
            </v-row>

            <v-row>
              <v-col cols="6">
                <v-container fluid>
                  <v-select :items="type" :disabled="!rent.RN_END_TIME" v-model="rent.R_TYPE" label="ประเภทห้อง"
                            dense></v-select>
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
                <v-text-field label="ราคารวม" v-model="totalPrice" readonly></v-text-field>
              </v-col>
            </v-row>
          </v-card-text>

          <!-- Button -->
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" :disabled="totalPrice === 0" text nuxt @click="checkRoom">
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
          <v-card-title> หลักฐานการโอนเงิน</v-card-title>
          <v-card-text class="d-flex justify-center mt-4">
            <v-row>
              <v-col cols="12" class="d-flex justify-center"
              ><img src="~/assets/Pic02.jpg" height="300"
              /></v-col>

              <v-col cols="12"
              >

                  <v-file-input
                    v-model="rent.RN_PAYMENT"
                    label="รูปภาพ"
                    show-size
                    counter
                    small
                    small-chips
                    ref="imgRef"
                  ></v-file-input
                  >

              </v-col>
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
              :disabled="!rent.RN_PAYMENT"
              @click="submit"
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
  name : 'index',
  async asyncData( { store, $axios } ) {
    store.dispatch( 'Auth/setAuthTrue' )
    // await $axios.$put( '/room/check-time-out' )
    const rooms = await $axios.$get( '/room/all' )
    // get Room name by room type
    const roomType = await rooms.map( room => room.R_TYPE )
    const type = await roomType.map( type => {
      switch ( type ) {
        case 'sm':
          return 'ห้องเล็ก'
        case 'md':
          return 'ห้องกลาง'
        case 'lg':
          return 'ห้องใหญ่'
      }
    } )

    return { rooms, type }

  },

  data() {
    return {
      dialog : false,
      snackbar : false,
      date : null,
      startTime : null,
      endTime : null,
      datePicker : false,
      startTimePicker : false,
      endTimePicker : false,
      file : null,
      price : 0,
      totalPrice : 0,
      RN_START_TIME : '',
      RN_END_TIME : '',

      telRules : [
        v => !!v || 'กรุณากรอกเบอร์โทรศัพท์',
        v => v.length === 10 || 'กรุณากรอกเบอร์โทรศัพท์ให้ครบ 10 หลัก',
        v => /^\d+$/.test( v ) || 'กรุณากรอกเบอร์โทรศัพท์เป็นตัวเลขเท่านั้น'
      ],

      rent : {
        CUS_NAME : '',
        CUS_TEL : '',
        RN_DATE : '',
        RN_START_TIME : '',
        RN_END_TIME : '',
        R_TYPE : '',
        RN_PAYMENT : null,
      },

      rImage : [
        require( '~/assets/show01.png' ),
        require( '~/assets/show02.png' ),
        require( '~/assets/show03.png' ),
        require( '~/assets/show04.png' ),
        require( '~/assets/show05.png' ),
        require( '~/assets/show06.png' ),
      ],
    }
  },

  methods : {
    async submit() {
      const formData = new FormData()
      formData.append( 'img', this.rent.RN_PAYMENT )
      formData.append( 'RN_DATE', this.rent.RN_DATE )
      formData.append( 'RN_START_TIME', this.rent.RN_START_TIME )
      formData.append( 'RN_END_TIME', this.rent.RN_END_TIME )
      formData.append( 'R_TYPE', this.rent.R_TYPE )
      formData.append( 'CUS_NAME', this.rent.CUS_NAME )
      formData.append( 'CUS_TEL', this.rent.CUS_TEL )
      formData.append('TotalPrice', this.totalPrice)

      const rent = await this.$axios.$post( '/rent/', formData )
      this.dialog = false
      this.$swal( {
        title : 'จองห้องสำเร็จ',
        text : 'จองห้องใน วัน-เวลา ที่เลือกสำเร็จ',
        type : 'success',
      } )
    },

    async checkRoom(){
      const check = await this.$axios.$post('/rent/check-room/', {
        RN_DATE: this.rent.RN_DATE,
        RN_START_TIME: this.rent.RN_START_TIME,
        RN_END_TIME: this.rent.RN_END_TIME,
        R_TYPE: this.rent.R_TYPE
      })
      console.log(check)
      if(check.isAvailable){
        this.dialog = true
      }else {
        this.$swal( 'ห้องถูกใช้งาน', 'ห้องที่ท่านเลือกไม่ว่างในเวลานั้น', 'error' )
      }
    },




    calTotalPrice() {
     // if null
      if ( !this.rent.RN_DATE|| !this.rent.RN_START_TIME|| !this.rent.RN_END_TIME || !this.rent.R_TYPE ) {
        console.log( 'null' )
        this.totalPrice = 0
        return
      }
      console.log( 'not null' )
      // convert string to date
      const startDate = new Date( this.rent.RN_DATE + ' ' + this.rent.RN_START_TIME)
      const endDate = new Date( this.rent.RN_DATE + ' ' + this.rent.RN_END_TIME )
      // calculate time
      const time = (endDate.getTime() - startDate.getTime()) / 1000 / 60 / 60
      // ceil time
      const ceilTime = Math.ceil( time )
      this.totalPrice = ceilTime * this.price
    }
  },

  watch : {
    'rent.R_TYPE'() {
      switch ( this.rent.R_TYPE) {
        case 'ห้องเล็ก':
          this.price = this.rooms[0].R_PRICE
          break
        case 'ห้องกลาง':
          this.price = this.rooms[1].R_PRICE
          break
        case 'ห้องใหญ่':
          this.price = this.rooms[2].R_PRICE
          break
      }
      this.calTotalPrice()
    },
    'rent.RN_DATE'() {
      this.calTotalPrice()
    },
    'rent.RN_START_TIME'() {
      this.calTotalPrice()
    },
    'rent.RN_END_TIME'() {
      this.calTotalPrice()
    },
  }
}
</script>
<style lang="scss"></style>
