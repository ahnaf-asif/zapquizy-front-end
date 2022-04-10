<template>
  <v-container style="display:flex;align-items:center;justify-content: center;" fill-height>
      <div class="loginform">
        <v-card shaped>
<!--          {{prevRoute}}-->
          <v-card-title>Sign In ZapQuizy</v-card-title>
          <v-divider></v-divider>
          <v-card-text>
            <v-alert v-if="unauthorized" class="my-3" type="error">{{errMsg}}</v-alert>
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
      async login(){
        if(this.$refs.form.validate()){
          this.disableBtn = true;
          try{
            const formData = {
              email: this.email,
              password: this.password
            };
            const resp = await this.$auth.loginWith('laravelSanctum', {
              data: {
                email: this.email,
                password: this.password
              }
            });
            // const resp = await this.$auth.$get('/api/hudai');
            // console.log(resp);
            await this.$router.push({
              path: this.prevRoute.fullPath
            });

          }catch(err){
            this.disableBtn = false;
            this.unauthorized = true;
            this.errMsg = "Incorrect email or password";
            //console.log(`Error with ${err}`);
          }
        }
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
