<template>
    <div class="white">
      <v-dialog persistent v-model="phoneVerificationDialog">
        <v-card>
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
      <v-card color="card">
        <v-card-title class=" d-flex justify-center">
          <h1 class="text-h4 my-5">Sign In</h1>
        </v-card-title>
        <v-card-text>
          <v-alert v-if="unauthorized" class="my-3" type="error">{{errMsg}}</v-alert>
          <p v-if="phoneVerificationError" class="red--text darken-1 mb-3">Your phone number is not verified yet. Please
            <span class="blue--text darken-1" @click="phoneVerification()" style="cursor: pointer;">Click here</span> to verify.
          </p>
          <v-form ref="form">
            <v-text-field
              class=""
              append-icon="mdi-phone-outline"
              v-model="phone"
              label="Phone"
              :error-messages="phoneError"
              outlined
              required
            >
            </v-text-field>
            <v-text-field
              v-model="password"
              :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
              :type="show1 ? 'text' : 'password'"
              name="input-10-1"
              label="Password"
              :error-messages="passwordError"
              outlined
              @click:append="show1 = !show1"
            ></v-text-field>
            <p>Not Registered Yet? <span class="primary--text" @click="$nuxt.$emit('open-register-form')">Register Here</span></p>
            <div class="text-center mt-5">
              <v-btn @click="login" :disabled="disableBtn" color="primary" depressed elevation="2" large>Sign In</v-btn>
            </div>
          </v-form>
        </v-card-text>
      </v-card>
    </div>
</template>

<script>

export default {
  components: {
  },
  data(){
    return {
      valid: true,
      phone: '',
      phoneRules: [
        v => !!v || 'Phone is required'
      ],

      password: '',
      passwordRules: [
        v => !!v || 'Password is required',
        v => (v && v.length >= 8) || 'Password Must be at least 8 characters long',
      ],
      show1: false,
      prevRoute: {
        fullPath: '/',
      },
      unauthorized: false,
      errMsg: null,
      disableBtn: false,
      phoneError: null,
      passwordError : null,

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

  methods: {
    resetData(){
      this.disableBtn = false;
      this.phone = null;
      this.password = null;
      this.phoneVerificationError = false;
    },
    validateData(){
      let validXX = true;
      if(!this.phone){
        this.phoneError = 'Phone is required';
        validXX = false;
      }
      if(!this.password){
        this.passwordError='Password is required';
        validXX = false;
      }
      return validXX;
    },
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
    async login(){
      this.phoneError = null;
      this.passwordError = null;
      if(this.validateData()){
        this.unauthorized = false;
        this.errMsg=null;
        this.disableBtn = true;
        try{
          const formData = {
            phone: this.phone,
            password: this.password,
            verify: false,
          };
          const user = await this.$axios.$post('/api/check-phone-verification', formData);
          if(user.length  && user[0].phone_verified){
            try{
              const resp = await this.$auth.loginWith('laravelSanctum', {
                data: formData
              });
              // console.log('here login is happening')
              this.resetData();
              this.$nuxt.$emit('close-login-form');
            }catch(err){
              this.disableBtn = false;
              this.unauthorized = true;
              this.errMsg = "Incorrect phone or password";
            }
          }
          else{

          }


        }catch(err){
          this.disableBtn = false;
          console.log(`error hoise: ${err}`);
        }
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
            this.resetData();
            this.$nuxt.$emit('close-login-form');
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

<style lang="scss">
//@media(max-width: 760px){
//  .loginform{
//    width: 100vw;
//  }
//}
//@media(min-width: 760px){
//  .loginform{
//    width: 500px;
//  }
//}
</style>
