<template>
<q-page class="bg-teal window-height:100% row justify-center items center ">
  <q-img src="~assets/lolz.jpg" class="menu-image fixed-center" />

 <div class="column q-pa-md">
      <div class="row">
        <q-card square class="shadow-24" style="width:415px;height:470px;">

              <div class="q-pa-xs">
        <q-img src="~assets/z.png"/></div>

          <q-card-section>
              <q-form
                  @submit="onSubmit"
                  class="q-gutter-md">

          <q-input
            bg-color="grey-4"
            filled
            v-model="formData.room_no"
            label="Input Room Number *"
          />

        <div class="row">
        <div class="q-pa-xs">
          <q-select
            color="teal"
            bg-color="grey-4"
            filled style=" width:270px;"
            v-model="formData.time_in"
            :options="options"
            label="Time In *"/>
            </div>
        <div class="q-pa-xs">
        <q-select
            color="teal"
            bg-color="grey-4"
            filled style=" width:270px;"
            v-model="formData.time_out"
            :options="options"
            label="Time Out *"/>
          </div>
        </div>

        <q-input
            bg-color="grey-4"
            filled
            v-model="formData.teacher"
            label="Teacher *"
          />

          <div class="q-pa-xs">
            <q-btn  style="width:100px;height:40px;" color="teal-10" unelevated size="md" label="Update" @click="addRoom()"/>
          </div>
          </q-form>
         </q-card-section>
       </q-card>
      </div>
    </div>

  <q-footer >
      <q-tabs class="" style="height:50px;">
        <q-route-tab class="bg-teal" to="/school" label="School Input" style="width:100%;height:60px;"/>
        <q-route-tab class="bg-teal-10" to="/history" label="History"  style="width:100%;height:60px;"/>
      </q-tabs>

  </q-footer>
</q-page>
</template>

<script>
import { mapState } from 'vuex'
import { firebaseDb } from 'src/boot/firebase'
export default {
  name: 'SignupPage.vue',
  data () {
    return {
      options: [
        '8:00 AM', '9:00 AM', '10:00 AM', '11:00 AM', '12:00 PM', '1:00 PM', '2:00 PM', '3:00 PM', '4:00 PM', '5:00 PM', '6:00 PM', '7:00 PM'
      ],
      result: null,
      step: 1,
      formData: {
        room_no: '',
        time_in: '',
        time_out: '',
        teacher: ''
      }
    }
  },
  computed: {
    ...mapState('store', ['userDetails'])
  },
  methods: {
    addRoom () {
      firebaseDb.ref('users/' + this.userDetails.userId + '/rooms').push({
        room_num: this.formData.room_no,
        time_in: this.formData.time_in,
        time_out: this.formData.time_out,
        teacher: this.formData.teacher
      })
      this.formData.room_no = ''
      this.formData.time_in = ''
      this.formData.time_out = ''
      this.formData.teacher = ''
      this.$q.notify('New Room Added')
    }
  }
}
</script>

<style>
</style>
