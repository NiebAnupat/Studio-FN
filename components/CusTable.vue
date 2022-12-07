<template>
  <div>
    <v-simple-table fixed-header height="350px">
      <template v-slot:default>
        <thead>
        <tr>
          <th class="text-center">ชื่อลูกค้า</th>
          <th class="text-center">เบอร์โทรศัพท์</th>
          <th class="text-center">ประเภทห้อง</th>
          <th class="text-center">วันที่</th>
          <th class="text-center">เวลาเริ่ม</th>
          <th class="text-center">เวลาสิ้นสุด</th>
          <th class="text-center"></th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="item in rents" :key="item.RN_ID">
          <td class="text-center">{{ item.customer.CUS_NAME }}</td>
          <td class="text-center">{{ item.customer.CUS_TEL }}</td>
          <td class="text-center">{{ getTypeName(item.using_room[0].room.R_TYPE) }}</td>
          <td class="text-center">{{ formatDate( item.RN_DATE ) }}</td>
          <td class="text-center">{{ formatTime( item.RN_START_TIME ) }}</td>
          <td class="text-center">{{ formatTime( item.RN_END_TIME ) }}</td>
          <td class="text-center pa-2">
            <v-tooltip top>
              <template v-slot:activator="{ on, attrs }">
                <v-btn
                  icon
                  v-bind="attrs"
                  v-on="on"
                  nuxt
                  @click="showImg(item.RN_PAYMENT,item.RN_PRICE)"
                >
                  <v-icon color="grey" class="mx-1"> mdi-book-search</v-icon>
                </v-btn>
              </template>
              <span>สลิป</span>
            </v-tooltip>

            <v-tooltip top>
              <template v-slot:activator="{ on, attrs }">
                <v-btn icon v-bind="attrs" v-on="on" @click="rejectRent(item.RN_ID)">
                  <v-icon color="red" class="mx-1"> mdi-minus</v-icon>
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
          <v-card-title> หลักฐานโอนเงิน</v-card-title>
          <v-card-text class="d-flex justify-center mt-4">
            <v-row class="d-flex justify-center"
            >
              <v-img :src="img" width="200"/>
              <v-text-field
                v-model="total"
                label="ราคารวม"
                readonly
              ></v-text-field
              >
            </v-row>
          </v-card-text>
        </v-card>
      </v-dialog>
    </div>
  </div>
</template>
<script>
import moment from 'moment'

export default {
  props : {
    rents : {
      type : Array,
      required : true,
      asyncData : true
    },
    refreshData : {
      type : Function,
      required : true,
      asyncData : true
    }
  },
  data() {
    return {
      dialog : false,
      img : null,
      total : 2000,
    }
  },
  methods : {

    rejectRent( id ) {
      this.$axios.$delete( `/rent/reject/${ id }` ).then( () => {
        this.refreshData()
      } )
    },

    formatDate( date ) {
      return moment( date ).format( 'DD/MM/YYYY' )
    },
    formatTime( time ) {
      return moment( time ).format( 'HH:mm' )
    },
    showImg( img, total ) {
      this.img = this.convertBlobToURL( img )
      this.total = total
      this.dialog = true;
    },
    convertBlobToURL( blob ) {
      return (
        'data:image/jpeg;base64,' +
        new Buffer(
          new Uint8Array( blob.data ).reduce(
            ( data, byte ) => data + String.fromCharCode( byte ),
            ''
          ),
          'binary'
        ).toString( 'base64' )
      )
    },
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
  },
}
</script>
<style lang="scss"></style>
