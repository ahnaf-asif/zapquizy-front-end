<template>
  <v-container fill-height class="nav-bar" style="display: block !important;position: relative !important;">
    <div>
      <div class="top">
        <div class="inside">
          <div class="text-center">
            <img style="border-radius: 50%;" width="100" src="/icon.png" alt="icon">
          </div>
          <h1 class="text-center text-h5 font-weight-bold">ZapQuizy</h1>
        </div>
      </div>
      <v-list>

        <v-list-item style="min-height: 35px !important;" >
          <v-btn color="transparent"  elevation="0"  to="/" active-class="nav-btn-active"> <v-icon>mdi-home-variant-outline</v-icon>&nbsp; Home</v-btn>
        </v-list-item>
        <v-list-item style="min-height: 35px !important;" v-for="link in links" :key="link.name">
          <v-btn elevation="0" color="transparent" active-class="nav-btn-active" :to="link.to"><v-icon>{{link.icon}}</v-icon> &nbsp; {{ link.name }}</v-btn>
        </v-list-item>

<!--        // guest-->
        <v-list-item style="min-height: 35px !important;" v-if="!$auth.user">
          <v-btn active-class="nav-btn-active" color="transparent" elevation="0"  @click="openLoginForm()"> <v-icon>mdi-login-variant</v-icon>&nbsp; Sign In</v-btn>
        </v-list-item>
        <v-list-item style="min-height: 35px !important;" v-if="!$auth.user">
          <v-btn elevation="0" color="transparent" active-class="nav-btn-active" @click="openRegisterForm()"><v-icon>mdi-account-plus-outline</v-icon>&nbsp; Register</v-btn>
        </v-list-item>
<!--        user-->
        <v-list-item style="min-height: 35px !important;" v-if="$auth.user">
          <v-btn elevation="0" color="transparent" active-class="nav-btn-active" to="/profile/"> <v-icon>mdi-account-school-outline</v-icon>&nbsp; Profile</v-btn>
        </v-list-item>
        <v-list-item style="min-height: 35px !important;" v-if="$auth.user">
          <v-btn elevation="0" color="transparent" active-class="nav-btn-active" @click="logout()"> <v-icon>mdi-arrow-left-thin-circle-outline</v-icon>&nbsp; Sign Out</v-btn>
        </v-list-item>
      </v-list>

    </div>
    <div class="navbar-bottom-hoga text-center text-xs" style="
      position: absolute !important;
      text-align: center;
      margin-left: auto !important;
      margin-right: auto !important;
      left: 10px !important;
      right: 0 !important;
      bottom: 10px !important;
      color: grey;
    ">
      ZapQuizy 1.0
    </div>
  </v-container>
</template>

<script>
import NavProfileSection from './NavProfileSection';
export default {
  name: "MobileNavLinks.vue",
  props: ['links'],
  components: {
    NavProfileSection
  },
  methods: {
    async logout(){
      await this.$auth.logout();
      await this.$router.push('/');
      this.$emit('close-sidebar');
    },
    openLoginForm(){
      this.$nuxt.$emit('open-login-form');
    },
    openRegisterForm(){
      this.$nuxt.$emit('open-register-form');
    }
  }
}
</script>

<style lang="scss" scoped>
  .navlink {
    //color: black;
    text-decoration: none;
    cursor: pointer;
    &:hover {
      color: #7368ed;
    }
  }
  .top{
    min-height: 250px !important;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .nav-btn-active{
    background: transparent !important;
    color: #7368ed !important;
    &::before{
      opacity: 0;
    }
  }
</style>
