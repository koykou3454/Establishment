<template>
  <q-page class="bg-teal row justify-center items-center">
  <q-img src="~assets/lolz.jpg" class="menu-image fixed-center" />
    <div class="column q-pa-lg">
      <div class="row">
        <q-card square class="shadow-24" style="width:300px;height:450px;">
          <q-card-section class="bg-teal-10">

       <div class="q-pa-xs q-my-sm">
        <q-img src="~assets/z.png"/>
       </div>
          </q-card-section>

          <q-card-section>
            <q-form
                  @submit="onSubmit"
                  class="q-px-sm q-pt-sm">

              <q-input v-model="formData.email" color="teal" type="email" label="Email">
                <template v-slot:prepend>
                  <q-icon name="mail" />
                </template>
              </q-input>

              <q-input v-model="formData.password" color="teal" label="Password" :type="isPwd ? 'password' : 'text'">
                <template v-slot:prepend>
                    <q-icon name="lock" />
                </template>
                <template v-slot:append>
                  <q-icon
                    :name="isPwd ? 'visibility_off' : 'visibility'"
                    class="cursor-pointer"
                    @click="isPwd = !isPwd"/>
                </template>
              </q-input>

      <div class="q-pt-md">
          <q-card-actions class="q-pa-xs q-pt-lg">
            <q-btn type="submit"  unelevated size="lg" color="teal-10" class="full-width text-white" label="Sign In" />
          </q-card-actions>
      </div>

            </q-form>
          </q-card-section>

    <q-form class="q-px-xs q-pt-xs">
          <q-card-actions class="q-pa-md">
            <router-link v-bind:to="'/signup'"  class='text-grey-10'>Create an Account</router-link>
          </q-card-actions>
    </q-form>

        </q-card>
      </div>
    </div>

    <q-dialog v-model="popup" persistent>
      <q-card style="min-width: 350px">
        <q-card-section>
          <div class="text-h6">Request Successfull</div>
          <div class="text-h7">You will be updated via sms for your request</div>
        </q-card-section>

        <q-card-actions align="right" class="text-primary">
          <q-btn color="teal" flat label="Return" @click="$router.replace('/')" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>

  </q-page>
</template>

<script>
import { mapActions } from 'vuex'
import { firebaseAuth } from 'src/boot/firebase'
export default {
  name: 'LoginPage.vue',
  data () {
    return {
      getCookie (name) {
        const value = `; ${document.cookie}`
        const parts = value.split(`; ${name}=`)
        if (parts.length === 2) return parts.pop().split(';').shift()
      },
      popup: '',
      formData: {
        email: '',
        password: ''
      },
      isPwd: true
    }
  },
  methods: {
    ...mapActions('store', ['loginUser']),
    onSubmit () {
      this.loginUser(this.formData)
    }
  },

  mounted () {
    this.popup = Boolean(this.getCookie('requestAccess'))
    document.cookie.split(';').forEach(function (c) { document.cookie = c.replace(/^ +/, '').replace(/=.*/, '=;expires=' + new Date().toUTCString() + ';path=/') })
  }
  // mounted () {
  //   firebaseDb.ref('requests').push({
  //     name: "D'Morvie",
  //     type: "",
  //     level: ,
  //     contact: ,
  //     contact1: ,
  //     address: ,
  //     password: ,
  //     email:
  //   })
  // }
}
</script>

<style lang="sass">
.menu-image
  height: 100%
  opacity: 0.2
  filter: grayscale(100%)

</style>
