<template>
  <v-container fill-height style="display: flex;justify-content: center;align-items: center;">
    <v-dialog persistent v-model="phoneVerificationDialog">
      <v-card >
        <v-card-title>Verify Phone number</v-card-title>
        <v-divider></v-divider>
        <v-card-text class="mt-3">
          <p>We have sent a <b>5 digit OTP code</b> to your phone. Please type or copy-paste that code below.</p>
          <v-otp-input
            length="5"
            v-model="otpInput"
            :disabled="otpCheckingLoading"
            @finish="onOtpInputFinish"
          ></v-otp-input>
          <p v-if="otpCheckingLoading" class="text-center blue--text darken-1">Checking OTP....</p>
          <p v-if="otpIncorrect" class="text-center red--text darken-1">Wrong OTP...try again</p>
          <p v-if="otpCorrect" class="text-center green--text darken-1">OTP correct...Logging in...</p>
        </v-card-text>
        <v-card-actions>
          <!--              <v-spacer></v-spacer>-->
          <!--              <v-btn color="error" @click="phoneVerificationDialog = false">Close</v-btn>-->
          <p v-if="!requestNewOtp">you can request new OTP Code in <span class="blue--text">{{ requestNewOtpCountdownShow }}</span></p>
          <p v-else class="text-center">
            <v-btn @click="requestNewVerificationClicked" x-small class="primary-bg-dark">
              {{ requestNewOtpBtnText }}
            </v-btn>
          </p>
        </v-card-actions>
      </v-card>
    </v-dialog>
      <div class="regiform">
        <v-card max-width="450" :elevation="$vuetify.breakpoint.mobile?0:3" >
          <h1 class="text-center mb-5 mb-md-1 text-h4 font-weight-bold">Register ZapQuizy</h1>

          <v-card-text>
            <v-form ref="form" v-model="valid" lazy-validation>
              <v-text-field
                outlined
                v-model="name"
                label="Name"
                :rules="nameRules"
                :error-messages="errorMessages.name"
                required
              >
              </v-text-field>

              <v-text-field
                outlined
                v-model="phone"
                label="Phone"
                :rules="phoneRules"
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
                @click:append="show1 = !show1"
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
      nameRules: [
        v => !!v || 'Name is required',
      ],
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
      phoneVerificationDialog: false,
      phoneVerificationError: false,
      otpCheckingLoading: false,
      otp: null,
      otpInput: '',
      otpIncorrect: false,
      otpCorrect: false,
      requestNewOtp: false,
      requestNewOtpCountdown: 3*60,
      requestNewOtpCountdownShow: '',
      requestNewOtpBtnText: 'Request new OTP code',
    }
  },
  beforeRouteEnter(to, from, next){
    next( vm=>{
      vm.prevRoute = from
    })
  },
  mounted(){
    this.$refs.scrollTop = 0;
    this.$axios.$get('/sanctum/csrf-cookie');
  },
  methods:{
    secondToTime(sec){
      let minutes = Math.floor(sec/60);
      let secs = sec - minutes*60;
      if(secs < 10){
        secs = `0${secs}`;
      }
      return `${minutes}:${secs}`;
    },
    generateOTP(){
      let digits='0123456789';
      let otp = '';
      for (let i = 0; i < 5;i++){
        otp += digits[Math.floor(Math.random()*10)];
      }
      return otp;
    },
    async submitForm(){
      this.disableBtn = true;
      try{
        let newUser = {
          name: this.name,
          phone: this.phone,
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
          await this.phoneVerification();
          // // console.log(resp);
          // try{
          //   const resp2 = await this.$auth.loginWith('laravelSanctum', {
          //     data: {
          //       email: this.email,
          //       password: this.password
          //     }
          //   });
          //   await this.$router.push({
          //     path: this.prevRoute.fullPath
          //   });
          // }catch(e){
          //   console.log(e);
          // }
        }
      }catch(e){
        this.disableBtn = false;
        this.disableBtn = false;
        console.log('error', e);
      }
    },
    async phoneVerification(){

      this.requestNewOtpCountdown = 3*60;
      this.requestNewOtpCountdownShow = this.secondToTime(this.requestNewOtpCountdown);
      this.requestNewOtp = false;
      this.phoneVerificationDialog = true;
      this.otp = this.generateOTP();
      // let msg = "আপনার অ্যাকাউন্ট ভ্যারিফাই করুন"
      this.otp = '12345';

      let requestNewVerificationInterval = setInterval(()=>{
        this.requestNewOtpCountdown--;
        this.requestNewOtpCountdownShow = this.secondToTime(this.requestNewOtpCountdown);
        if(this.requestNewOtpCountdown <= 0){
          this.requestNewOtp = true;
          clearTimeout(requestNewVerificationInterval);
        }
      }, 1000);
    },
    async requestNewVerificationClicked(){
      this.requestNewOtpBtnText = 'Requesting...';
      setTimeout(async () => {
        this.requestNewOtpBtnText = 'Request new OTP code';
        await this.phoneVerification();
      },500);
    },
    async onOtpInputFinish(){
      this.otpCheckingLoading = true;
      this.otpIncorrect = false;
      this.otpCorrect = false;
      setTimeout(async () => {
        if (this.otp === this.otpInput) {
          this.otpCheckingLoading = false;
          this.otpIncorrect = false
          this.otpCorrect = true;
          try {
            const formData = {
              phone: this.phone,
              password: this.password,
              verify: true,
            };
            const resp = await this.$auth.loginWith('laravelSanctum', {
              data: formData
            });
            await this.$router.push({
              path: this.prevRoute.fullPath
            });
          } catch (e) {
            console.log(e);
          }
        } else {
          this.otpCheckingLoading = false;
          this.otpIncorrect = true;
          this.otpCorrect = false;
        }
      }, 2000);
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
