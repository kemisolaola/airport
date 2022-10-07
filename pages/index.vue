<template>
  <v-row justify="center" align="center" class="login-bg">
    <v-col cols="12" sm="10" md="5">
      <v-card dark class="mx-5 pt-1 px-4 login-card">
        <p class="mt-5 mb-1">Can’t wait to fly private jets?</p>
        <span>Share your details below to be notified at launch.
          <v-form ref="form" v-model="valid" lazy-validation>
            <v-row class="mt-2">
              <v-col cols="6" class="py-0">
                <v-text-field v-model="name" :rules="nameRules" color="primary" outlined dense  placeholder="Full name">

                </v-text-field>
              </v-col>
              <v-col cols="6" class="py-0">
                <v-text-field v-model="phoneNumber" :rules="phoneNumberRules" color="primary" outlined dense
                  placeholder="Phone number">

                </v-text-field>
              </v-col>
              <v-col cols="12" class="py-0">
                <v-text-field v-model="email" :rules="emailRules" color="primary" outlined dense
                  placeholder="Email id">

                </v-text-field>
              </v-col>
              <v-col cols="12" class="py-0">
                <v-textarea v-model="message" :rules="messageRules" color="primary" height="70" outlined dense
                   placeholder="Your message">

                </v-textarea>
              </v-col>
              <v-col cols="12">
                <v-btn @click="validate" block color="primary" height="45" elevation="0" :loading="loading">Submit</v-btn>
              </v-col>
            </v-row>
          </v-form>
        </span>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data () {
    return {
      valid: true,
      name: '',
      nameRules: [
        v => !!v || 'Full name is required',
        v => (v && v.length >= 7) || 'Name cannot be less than 7 characters'
      ],
      email: '',
      emailRules: [
        v => !!v || 'Email is required',
        v => /.+@.+\..+/.test(v) || 'Email musr be valid'
      ],
      message: '',
      messageRules: [
        v => !!v || 'Message is required'
      ],
      phoneNumber: '',
      phoneNumberRules: [
        v => !!v || 'Phone number is required',
        v => Number.isInteger(Number(v)) || 'Phone number must be valid'
      ],
      loading: false
    }
  },
  methods: {
    validate () {
      this.$refs.form.validate()
      if (this.$refs.form.validate()) {
        this.loading = true
        setTimeout(() => {
          this.$router.push('/airport')
        }, 2000)
      }
    }
  }
}
</script>

<style>

</style>
