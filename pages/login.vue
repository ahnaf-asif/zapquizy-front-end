<template>
  <v-container class="mt-15">
    <v-row justify="center">
      <v-col cols="12" md="6">
        <v-card max-width="450" shaped>
          <v-card-title>Sign In ZapQuizy</v-card-title>
          <v-divider></v-divider>
          <v-card-text>
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
                <v-btn @click="login" color="primary" depressed elevation="2" large>Sign In</v-btn>
              </div>
              <div class="mt-10">
                Not Registered Yet? <NuxtLink to="/register/">Register Here</NuxtLink>
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
    auth: 'guest',
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
        }
    },
    mounted(){
        this.$refs.scrollTop = 0;
        console.log(this.$axios.$get('/sanctum/csrf-cookie'));
    },
    methods: {
      async login(){
        if(this.$refs.form.validate()){
          try{
            const formData = {
              email: this.email,
              password: this.password
            };
            const resp = await this.$auth.loginWith('laravelSanctum', {
              data: {
                email: 'ahnafshahriar92@gmail.com',
                password: 't1f2ca3b'
              }
            });
            // const resp = await this.$auth.$get('/api/hudai');
            console.log(resp);
            await this.$router.push({
              path: '/'
            });

          }catch(err){
            console.log(`Error with ${err}`);
          }
        }
      }
    },
}
</script>

<style lang="scss">

</style>
