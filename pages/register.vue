<template>
  <v-container fill-height style="display: flex;justify-content: center;align-items: center;">

      <div class="regiform">
        <v-card max-width="450" shaped>
          <v-card-title>Register | ZapQuizy</v-card-title>
          <v-divider></v-divider>
          <v-card-text>
            <v-form ref="form" v-model="valid" lazy-validation>
              <v-text-field
                v-model="name"
                label="Name"
                :error-messages="errorMessages.name"
                required
              >
              </v-text-field>
              <v-text-field
                v-model="email"
                label="Email"
                :error-messages="errorMessages.email"
                :rules="emailRules"
                required
              >
              </v-text-field>
              <v-text-field
                v-model="phone"
                label="Phone"
                :rules="phoneRules"
                :error-messages="errorMessages.phone"
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
                :error-messages="errorMessages.password"
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
                :error-messages="errorMessages.confirmPassword"
                counter
                @click:append="show2 = !show2"
              ></v-text-field>

              <div class="text-center mt-5">
                <v-btn :disabled="disableBtn" @click="submitForm" ref="registerBtn" color="primary" depressed elevation="2" large>Register</v-btn>
              </div>
            </v-form>
          </v-card-text>
        </v-card>
      </div>
  </v-container>
</template>

<script>
export default {
  auth: 'guest',
  middleware: 'guest',
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
    }
  },
  beforeRouteEnter(to, from, next){
    next( vm=>{
      vm.prevRoute = from
    })
  },
  mounted(){
    this.$refs.scrollTop = 0;
    this.$axios.$get('/sanctum/csrf-cookie')
  },
  methods:{
    async submitForm(){
      if(this.password !== this.confirmPassword){
        this.errorMessages.password = 'passwords must match';
        this.errorMessages.confirmPassword = 'passwords must match';
        return;
      }
      this.disableBtn = true;
      try{
        let newUser = {
          name: this.name,
          phone: this.phone,
          email: this.email,
          password: this.password
        };
        const resp = await this.$axios.$post('/register', newUser);
        if(resp.error){
          this.disableBtn = false;
          if(resp.errors.email){
            this.errorMessages.email = resp.errors.email;
          }
          if(resp.errors.phone){
            this.errorMessages.phone = resp.errors.phone;
          }
          if(resp.errors.name){
            this.errorMessages.name = resp.errors.name;
          }
        }
        else{
          // console.log(resp);
          try{
            const resp2 = await this.$auth.loginWith('laravelSanctum', {
              data: {
                email: this.email,
                password: this.password
              }
            });
            await this.$router.push({
              path: this.prevRoute.fullPath
            });
          }catch(e){
            console.log(e);
          }
        }
      }catch(e){
        this.disableBtn = false;
        console.log('error', e);
      }
    }
  },

}
</script>

<style scoped>
@media(max-width: 760px){
  .regiform{
    width: 100vw;
  }
}
@media(min-width: 760px){
  .regiform{
    width: 500px;
  }
}
</style>
