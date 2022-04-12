<template>
  <v-container style="display:flex;align-items:center;justify-content: center;" fill-height>
      <div class="loginform">
        <v-dialog persistent v-model="phoneVerificationDialog" max-width="450">
          <v-card>
            <v-card-title>Verify Phone number via OTP</v-card-title>
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
        <v-card shaped>
          <v-card-title>Sign In ZapQuizy</v-card-title>
          <v-divider></v-divider>
          <v-card-text>
            <v-alert v-if="unauthorized" class="my-3" type="error">{{errMsg}}</v-alert>
            <p v-if="phoneVerificationError" class="red--text darken-1 mb-3">Your phone number is not verified yet. Please
              <span class="blue--text darken-1" @click="phoneVerification()" style="cursor: pointer;">Click here</span> to verify.
            </p>
            <v-form ref="form" v-model="valid" lazy-validation>
              <v-text-field
                v-model="email"
                label="Email"
                :rules="emailRules"
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

              <div class="text-center mt-5">
                <v-btn @click="login" :disabled="disableBtn" color="primary" depressed elevation="2" large>Sign In</v-btn>
              </div>
              <div class="mt-10">
                Not Registered Yet? <NuxtLink to="/register/">Register Here</NuxtLink>
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
    components: {
    },
    data(){
        return {
          valid: true,
          email: '',
          emailRules: [
            v => !!v || 'E-mail is required',
            v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
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
    mounted(){
        this.$refs.scrollTop = 0;
        this.$axios.$get('/sanctum/csrf-cookie')
    },
    beforeRouteEnter(to, from, next){
      next( vm=>{
        vm.prevRoute = from
      })
    },
    methods: {
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
        if(this.$refs.form.validate()){
          this.unauthorized = false;
          this.errMsg=null;
          this.disableBtn = true;
          try{
            const formData = {
              email: this.email,
              password: this.password,
              verify: false,
            };
            let user_verification_status = await this.$axios.$post('/api/check-phone-verification', formData);
            if(!user_verification_status.error){
              try{
                const resp = await this.$auth.loginWith('laravelSanctum', {
                  data: formData
                });
                await this.$router.push({
                  path: this.prevRoute.fullPath
                });
              }catch(err){
                this.disableBtn = false;
                this.unauthorized = true;
                this.errMsg = "Incorrect email or password";
              }
            }
            else{
              this.phoneVerificationError = true;
              this.disableBtn = false;
            }
          }catch(err){
            console.log(`Error with ${err}`);
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
                email: this.email,
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

<style lang="scss">
@media(max-width: 760px){
  .loginform{
    width: 100vw;
  }
}
@media(min-width: 760px){
  .loginform{
    width: 500px;
  }
}
</style>
