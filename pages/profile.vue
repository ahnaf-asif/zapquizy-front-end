<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="6">
        <div class="mt-10">
          <v-card max-width="450" :elevation="$vuetify.breakpoint.mobile?0:1" class="py-md-5 px-md-5" >
            <h1 class="text-center mb-5 mb-md-1 text-h4 font-weight-bold">{{ user.name }}</h1>
            <v-card-text>
              <v-form ref="form" v-model="valid" lazy-validation>
                <v-text-field
                  outlined
                  v-model="user.name"
                  label="Name"
                  :rules="nameRules"
                  :error-messages="errorMessages.name"
                  required
                >
                </v-text-field>
                <v-text-field
                  outlined
                  disabled
                  v-model="user.phone"
                  label="Phone"
                  :rules="phoneRules"
                  :hint="'You can not change your phone number'"
                  :error-messages="errorMessages.phone"
                  required
                >
                </v-text-field>
                <v-text-field
                  outlined
                  v-model="password"
                  :rules="passwordRules"
                  :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                  :type="show1 ? 'text' : 'password'"
                  name="input-10-1"
                  label="Password"
                  hint="At least 8 characters"
                  :error-messages="errorMessages.password"
                  counter
                  @click:append="show1 = !show1"
                ></v-text-field>
                <div class="text-center">
                  <v-btn :disabled="disableBtn" @click="submitForm" ref="registerBtn" color="primary" depressed outlined large>Save Account</v-btn>
                </div>
              </v-form>
            </v-card-text>
          </v-card>
        </div>
      </v-col>
      <v-col cols="12" md="6">
        <div class="mt-10">
          <div>
            <v-tabs v-model="tab" show-arrows grow center-active>
              <v-tab>Dashboard</v-tab>
              <v-tab>Quiz History</v-tab>
              <v-tab>My packages</v-tab>
            </v-tabs>
            <v-tabs-items v-model="tab" class="mt-10">
              <v-tab-item>
                <ProfileDashboard></ProfileDashboard>
              </v-tab-item>
              <v-tab-item>
                <ProfileQuiz></ProfileQuiz>
              </v-tab-item>
              <v-tab-item>
                <ProfilePackage></ProfilePackage>
              </v-tab-item>
            </v-tabs-items>
          </div>
        </div>
      </v-col>
    </v-row>

  </v-container>
</template>

<script>
import ProfilePackage from "../components/ProfilePackage";
import ProfileDashboard from "../components/ProfileDashboard";
import ProfileQuiz from "../components/ProfileQuiz";

export default {
  name: "profile.vue",
  middleware: "auth",
  transition: 'fade',
  components: {
    ProfilePackage, ProfileDashboard, ProfileQuiz
  },
  data(){
    return {
      valid: true,
      nameRules: [
        v => !!v || 'Name is required',
      ],
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
      ],
      phoneRules: [
        v => !!v || 'Phone number is required',
      ],
      password: null,
      passwordRules: [
        v => !!v || 'Password is required',
        v => (v && v.length >= 8) || 'Password Must be at least 8 characters long',
      ],

      show1: false,
      show2: false,
      errorMessages: {
        name: null,
        email: null,
        password: null,
        phone: null,
        confirmPassword: null,
      },
      disableBtn: false,
      prevRoute: {
        fullPath: '/',
      },
      user: {
        name: null,
        phone: null,
        password: null,
      },
      tab: null,
    }
  },
  created(){
    this.user = this.$auth.user;
  },
  methods: {
    submitForm(){

    }
  }
}
</script>

<style scoped>

</style>
