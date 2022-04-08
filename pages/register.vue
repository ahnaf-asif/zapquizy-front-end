<template>
  <v-container class="mt-5">
    <v-row justify="center">
      <v-col cols="12" sm="6" md="6">
        <v-card max-width="450" shaped>
          <v-card-title>Register | ZapQuizy</v-card-title>
          <v-divider></v-divider>
          <v-card-text>
            <v-form ref="form" v-model="valid" lazy-validation>
              <v-text-field
                v-model="name"
                label="Name"
                required
              >
              </v-text-field>
              <v-text-field
                v-model="email"
                label="Email"
                :rules="emailRules"
                required
              >
              </v-text-field>
              <v-text-field
                v-model="phone"
                label="Phone"
                :rules="phoneRules"
                required
              >
              </v-text-field>
              <v-text-field
                v-model="password"
                :rules="passwordRules"
                :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                :type="show1 ? 'text' : 'password'"
                name="input-10-1"
                label="Password"
                hint="At least 8 characters"
                counter
                @click:append="show1 = !show1"
              ></v-text-field>
              <v-text-field
                v-model="confirmPassword"
                :rules="confirmPasswordRules"
                :append-icon="show2 ? 'mdi-eye' : 'mdi-eye-off'"
                :type="show2 ? 'text' : 'password'"
                name="input-10-1"
                label="Confirm Password"
                hint="At least 8 characters"
                counter
                @click:append="show2 = !show2"
              ></v-text-field>

              <div class="text-center mt-5">
                <v-btn @click="submitForm" color="primary" depressed elevation="2" large>Register</v-btn>
              </div>
            </v-form>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  transition: 'fade',
  name: "register.vue",
  data(){
    return {
      valid: true,
      name: '',

      email: '',
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
      ],
      phone: '',
      phoneRules: [
        v => !!v || 'Phone number is required',
      ],
      password: '',
      confirmPassword: '',
      passwordRules: [
        v => !!v || 'Password is required',
        v => (v && v.length >= 8) || 'Password Must be at least 8 characters long',
      ],
      confirmPasswordRules: [
        v => !!v || 'Confirm Password is required',
        v => (v && v.length >= 8) || 'Confirm Password Must be at least 8 characters long',
      ],

      show1: false,
      show2: false,

    }
  },
  mounted(){
    this.$refs.scrollTop = 0;
  },
  methods:{
    async submitForm(){
      
    }
  },

}
</script>

<style scoped>

</style>
