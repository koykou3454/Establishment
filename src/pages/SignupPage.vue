<template>
<q-page class="bg-teal window-height:100% row justify-center items center ">
  <q-img src="~assets/lolz.jpg" class="menu-image fixed-center" />
    <q-stepper
    style="width:500%;height:55px"
      v-model="step"
      ref="stepper"
      alternative-labels
      color="primary"
      animated
    >
      <q-step
        :name="1"
        icon="settings"
        :done="step > 1">
      </q-step>

      <q-step
        :name="2"
        icon="camera"
        :done="step > 2">
      </q-step>

      <q-step
        :name="3"
        icon="local_post_office">
      </q-step>
    </q-stepper>

 <div class="column q-pa-md" style="margin-left: 25%">
      <div class="row q-ma-xl">
        <div class = "row justify-end">
        <q-card square class="shadow-24" style="">
        <q-card-section class="bg-teal-10">
      <div class="q-pa-xs">
        <q-img src="~assets/z.png"/>
      </div>
          </q-card-section>

          <q-card-section>
              <q-form
                  @submit="onSubmit"
                  class="q-gutter-md">

          <q-input
            color="teal"
            bg-color="grey-4"
            filled
            v-model="formData.name"
            label="Establishment Name *"
            lazy-rules
            :rules="[ val => val && val.length > 0 || 'Please type something']"
          />

            <q-select
            color="teal"
            bg-color="grey-4"
            filled style=" width:270px;"
            v-model="formData.type"
            :options="options"
            label="Establishment Type *"/>

        <q-select
            color="teal"
            bg-color="grey-4"
            filled style=" width:270px;"
            v-model="formData.level"
            :options="Options"
            label="Level *"/>

           <q-input
            color="teal"
            bg-color="grey-4"
            filled
            v-model="formData.contact"
            label="Mobile Number *"
            hint="09xx-xxx-xxxx"
            lazy-rules
            :rules="[ val => val && val.length > 0 || 'Please type something']"
        />

         <q-input
            color="teal"
            bg-color="grey-4"
            filled
            v-model="formData.contact1"
            label="Hotline-Number *"
            hint="xxx-xxxx"
            lazy-rules
            :rules="[ val => val && val.length > 0 || 'Please type something']"
        />

       <q-input
        color="teal"
            bg-color="grey-4"
            filled
            v-model="formData.address"
            label="Your full address *"
            hint="Street, Baranggay, City, Province"
            lazy-rules
            :rules="[ val => val && val.length > 0 || 'Please type something']"
         />

        <q-input
            color="teal"
            bg-color="grey-4"
            filled
            v-model="formData.email"
            label="Establishment's Email *"
            lazy-rules
            :rules="[ val => val && val.length > 0 || 'Please type something']"
          />

        <q-input color="teal" bg-color="grey-4" v-model="formData.password" filled :type="isPwd1 ? 'password' : 'text'" label="Password *" hint="Combination of letters & numbers">
          <template v-slot:append>
            <q-icon
              :name="isPwd1 ? 'visibility_off' : 'visibility'"
              class="cursor-pointer"
              @click="isPwd1 = !isPwd1"/>
          </template>
        </q-input>

        <q-uploader
        url="http://localhost:4444/upload"
        color="teal-10"
        label="Business Permit"
        />

          <div class="q-pa-xs">
            <q-card-actions class="q-pa-sm flex flex-center">

            <q-btn
              style="width:400px;height:40px;"
              type="submit"
              unelevated
              size="md"
              color="teal-10"
              class="text-white"
              label="Continue"
            />
            <q-btn to="/" style="width:400px;height:40px;" flat unelevated size="md" label="Back" />
            <div id="recaptcha-container"></div>
            </q-card-actions>
           </div>
          </q-form>
         </q-card-section>
       </q-card>
       <div style="margin-top: 30%" class=" q-ma-xl">
         <q-card>
           <div style="color: #004d40" class="text-h6 text-weight-bold q-ml-xs q-pl-sm q-pt-sm">Types of Level</div>
          <q-list>
            <div style="color: #004d40" class="text-h7 text-weight-bold q-ml-xs q-pl-sm q-pt-sm">Establishment </div>
              <q-item>
                <q-item-section>
                  <q-item-label>Level I</q-item-label>
                  <q-item-label caption lines="2">Mall</q-item-label>
                </q-item-section>
              </q-item>

              <q-item>
                <q-item-section>
                  <q-item-label>Level II</q-item-label>
                  <q-item-label caption lines="2">Department Store, Restaurant</q-item-label>
                </q-item-section>
              </q-item>

              <q-item>
                <q-item-section>
                  <q-item-label>Level III</q-item-label>
                  <q-item-label caption lines="2">Small stores inside Mall, Food Stalls and etc.</q-item-label>
                </q-item-section>
              </q-item>

            <div style="color: #004d40" class="text-h7 text-weight-bold q-ml-xs q-pl-sm q-pt-sm">School </div>
              <q-item>
                <q-item-section>
                  <q-item-label>Level I</q-item-label>
                  <q-item-label caption lines="2">Whole Campus or University</q-item-label>
                </q-item-section>
              </q-item>

              <q-item>
                <q-item-section>
                  <q-item-label>Level II</q-item-label>
                  <q-item-label caption lines="2">Colleges inside the University</q-item-label>
                </q-item-section>
              </q-item>

              <q-item>
                <q-item-section>
                  <q-item-label>Level III</q-item-label>
                  <q-item-label caption lines="2">Rooms</q-item-label>
                </q-item-section>
              </q-item>

            </q-list>
          </q-card>
       </div>
       </div>
      </div>
    </div>

    <q-dialog v-model="otpConfirm" persistent>
      <q-card style="min-width: 350px">
        <q-card-section>
          <div class="text-h6">Enter OTP</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          <q-input dense v-model="otpCode" autofocus @keyup.enter="prompt = false" />
        </q-card-section>

        <q-card-actions align="right" class="text-primary">
          <q-btn flat label="Cancel" v-close-popup />
          <q-btn flat label="Enter" @click="confirmOTP()" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
</q-page>
</template>

<script>
import { firebaseAuth, firebaseDb, firebase } from 'src/boot/firebase'
import { mapState, mapActions } from 'vuex'
export default {
  name: 'SignupPage.vue',
  data () {
    return {
      step: 1,
      otpConfirm: false,
      otpCode: '',
      formData: {
        name: '',
        type: '',
        level: '',
        contact: '',
        contact1: '',
        address: '',
        password: '',
        email: ''
      },
      isPwd1: true,
      select: null,
      requestSuccess: true,
      options: [
        'Mall', 'Bank', 'Church', 'City Hall', 'School', 'Market', 'Supermarket', 'Department Store', 'Convenience Store', 'Hotel', 'Hospital', 'Government Office', 'Restaurant', 'Pharmacy/Drugstore', 'Other'
      ],
      Select: null,
      Options: [
        'Level I', 'Level II', 'Level III'
      ]
    }
  },
  methods: {

    ...mapActions('store', ['logoutUser']),

    goBack () {
      this.requestSuccess = true
    },

    confirmOTP () {
      window.confirmationResult.confirm(this.otpCode).then((result) => {
        // User signed in successfully.
        // this.registerUser(this.formData)
        firebaseDb.ref('requests').push({
          name: this.formData.name,
          type: this.formData.type,
          level: this.formData.level,
          contact: this.formData.contact,
          contact1: this.formData.contact1,
          address: this.formData.address,
          password: this.formData.password,
          email: this.formData.email
        })
        this.goBack()
        document.cookie = 'requestAccess = true'
        this.logoutUser()
        // ...
      }).catch((error) => {
        // User couldn't sign in (bad verification code?)
        // ...
        alert(error)
      })
    },
    onSubmit () {
      const phoneNumber = '+63' + this.formData.contact
      alert(phoneNumber)
      const appVerifier = window.recaptchaVerifier
      firebaseAuth.signInWithPhoneNumber(phoneNumber, appVerifier)
        .then((confirmationResult) => {
          // SMS sent. Prompt user to type the code from the message, then sign the
          // user in with confirmationResult.confirm(code).
          window.confirmationResult = confirmationResult
          alert('SMS sent')
          this.otpConfirm = true
          // ...
        }).catch((error) => {
          alert(error)
        })
      // this.registerUser(this.formData)
    }
  },
  mounted () {
    firebaseAuth.useDeviceLanguage()
    window.recaptchaVerifier = new firebase.auth.RecaptchaVerifier('recaptcha-container')
  }
}
</script>

<style>
</style>
