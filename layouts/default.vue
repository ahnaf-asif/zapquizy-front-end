<template>
  <v-app>
<!--    <div class="top-nav blue-grey darken-4 py-2">-->
<!--      <div class="inside d-flex align-center justify-space-between mx-2" >-->
<!--        <div class="contacts">-->
<!--          <v-icon style="font-size: 15px;color:whitesmoke">mdi-phone</v-icon>+8801755884404 &nbsp;&nbsp;-->
<!--          <v-icon style="font-size: 15px;color:whitesmoke">mdi-email-outline</v-icon> ahnafshahriar92@gmail.com-->
<!--        </div>-->
<!--        <div class="login">-->
<!--          <v-icon style="font-size: 15px;color:whitesmoke;">mdi-arrow-right-bold-box-outline</v-icon> Login-->
<!--        </div>-->
<!--      </div>-->
<!--    </div>-->
      <v-app-bar v-if="!$vuetify.breakpoint.mobile" :clipped-left="clipped" app fixed flat elevation="0">
          <v-container class="pa-0  fill-height">

            <NuxtLink class="nav-link-item nav-title-link " to="/">
              <v-toolbar-title v-text="'ZapQuizy'" class="font-weight-bold nav-title text-h4 " />
            </NuxtLink>
            <v-spacer />
            <DesktopNavLinks :links="links"></DesktopNavLinks>
          </v-container>
      </v-app-bar>

      <v-system-bar color="card" fixed v-if="$vuetify.breakpoint.mobile" height="40">
        <v-icon style="font-size: 25px;" @click.stop="rightDrawer = !rightDrawer">mdi-menu</v-icon>
        <v-spacer></v-spacer>
        <v-switch
          v-model="$vuetify.theme.dark"
        >
          <template v-slot:label>
            Theme
          </template>
        </v-switch>
      </v-system-bar>

    <v-main>
        <Nuxt />
    </v-main>
<!--    {{ $route.path }}-->
    <div v-if="$vuetify.breakpoint.mobile" class="decoy" style="height: 60px;width: 100%;">

    </div>

    <v-navigation-drawer v-if="$vuetify.breakpoint.mobile" v-model="rightDrawer" :right="right" temporary fixed>
      <MobileNavLinks v-on:close-sidebar="closeSidebar()" :links="links"></MobileNavLinks>
    </v-navigation-drawer>

    <v-dialog v-model="loginDialog" max-width="450" >
      <LoginComponent></LoginComponent>
    </v-dialog>
    <v-dialog v-model="registerDialog" max-width="450" >
      <RegisterComponent></RegisterComponent>
    </v-dialog>


    <v-bottom-navigation v-if="$vuetify.breakpoint.mobile" shift fixed>
<!--      <v-btn @click.stop="rightDrawer = !rightDrawer" >-->
<!--        <v-icon>mdi-menu</v-icon>-->
<!--      </v-btn>-->

      <v-btn active-class="primary--text" to="/">
        <span>home</span>
        <v-icon>mdi-home</v-icon>
      </v-btn>
      <v-btn active-class="primary--text" to="/model-test/" >
        <span>Model Test</span>
        <v-icon>mdi-book-open-outline</v-icon>
      </v-btn>
      <v-btn active-class="primary--text" to="/quiz/" >
        <span>Live Exams</span>
        <v-icon>mdi-ballot-outline</v-icon>
      </v-btn>
      <v-btn active-class="primary--text" to="/profile/" >
        <span>Profile</span>
        <v-icon>mdi-account</v-icon>
      </v-btn>
    </v-bottom-navigation>

<!--    <v-footer :absolute="!fixed" app color="blue-grey darken-4 white&#45;&#45;text" class="py-10">-->
<!--      <v-row>-->
<!--        <v-col cols="12" md="5">-->
<!--          <h1 class="mb-10">Let's work Together</h1>-->
<!--          <p style="max-width: 50ch;">Our club is open for School, College and University going students all over the country. We work on different olympiads, Scientific Magazine, publishing blogs and many other things. We organize Mega festivals, various Olympiads and competitions. We also organize camps and seminars.</p>-->
<!--          <p>Please Join us Here if you haven't already.</p>-->
<!--        </v-col>-->
<!--        <v-col cols="12" md="3">-->
<!--          <h1 class="mb-10 text-center">Quick Links</h1>-->
<!--          <v-row>-->
<!--            <v-col cols="6" style="">-->
<!--              <ul>-->
<!--                <li>List Item 1</li>-->
<!--              </ul>-->
<!--            </v-col>-->
<!--            <v-col cols="6" style="text-align:right;">-->
<!--              <ul class="" >-->
<!--                <li>List item 2</li>-->
<!--              </ul>-->
<!--            </v-col>-->
<!--          </v-row>-->
<!--        </v-col>-->
<!--        <v-col>-->
<!--          <h1 class="text-center mb-10">Contact Us</h1>-->
<!--          <p class="text-center">+880 1633923851</p>-->
<!--          <p class="text-center">bosonbiggansangho@gmail.com</p>-->
<!--          <p class="text-center">Leave us a message here.</p>-->
<!--        </v-col>-->
<!--      </v-row>-->
<!--    </v-footer>-->
  </v-app>
</template>

<script>
import DesktopNavLinks from '/components/DesktopNavLinks';
import MobileNavLinks from '/components/MobileNavLinks';
import LoginComponent from "../components/LoginComponent";
import RegisterComponent from "../components/RegisterComponent";

export default {
  auth: false,
  components: {MobileNavLinks, DesktopNavLinks, LoginComponent, RegisterComponent},
  transition: 'fade',
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      rightDrawer: false,
      loginDialog: false,
      registerDialog : false,
      right: false,
      title: '⚡ ZapQuizy',
      links: [
        {name: 'Model Test', to: '/model-test/', icon: 'mdi-clipboard-text-multiple-outline'},
        {name: 'Quiz', to: '/quiz/', icon: 'mdi-clock-edit-outline'},
      ],
      bottomNav: 'home',
    }
  },
  methods: {
    async closeSidebar(){
      await this.$router.push({
        path: '/'
      });
      this.rightDrawer = false;
      // console.log('drawer closed');
    },
    openLoginForm(){
      this.rightDrawer = false;
      this.registerDialog = false;
      this.loginDialog = true;
    },
    closeLoginForm(){
      this.rightDrawer = false;
      this.registerDialog = false;
      this.loginDialog = false;
    },
    openRegisterForm(){
      this.rightDrawer = false;
      this.registerDialog = true;
      this.loginDialog = false;
    },
    closeRegisterForm(){
      this.rightDrawer = false;
      this.registerDialog = false;
      this.loginDialog = false;
    }
  },
  mounted(){
    // if(process.browser){
    //   if(window.MathJax){
    //     window.MathJax.Hub.Queue([
    //       "Typeset",
    //       window.MathJax.Hub,
    //       this.$refs.equationEnabled,
    //     ]);
    //   }
    // }
  },
  created(){
    this.$nuxt.$on('open-login-form', ()=>{
      this.openLoginForm();
    });
    this.$nuxt.$on('close-login-form', ()=>{
      this.closeLoginForm();
    });
    this.$nuxt.$on('open-register-form', ()=>{
      this.openRegisterForm();
    });
    this.$nuxt.$on('close-register-form', ()=>{
      this.closeRegisterForm();
    });

  },
}
</script>


<style lang="scss">
.top-image{
  height: 40px;
  width: 40px;
  //border: 2px solid #6A1B9A;
  //border-radius: 50%;
  //background-color: lightgray;
  //background-image: url("/images/user.png");
  //background-size:     cover;
  //background-repeat:   no-repeat;
  //background-position: center center;
}
.bottom-nav{
  width: 100%;
  height: 60px;
  position: fixed;
  margin: 0 !important;
  padding: 0 !important;
  bottom: -3px !important;
  left: 0;
  display: flex;
  align-items: center;
  justify-content: space-around;
  a{
    text-decoration: none !important;
  }
  .v-icon{
    font-size: 30px !important;
    //text-decoration: underline;
  }
}
.top-nav{
  position: fixed;
  font-size: 12px;
  color: whitesmoke;
  z-index: 6;
  top: 0;
  left: 0;
  right: 0;
  text-transform: none !important;
}
.nav-link{
  display: flex;
  padding: 0;
  margin: 0;
  a{
    //color: black;
    text-decoration: none;
    &:hover{
      color: #7368ed;
    }
  }
}
.nav-title-link{

  // text-decoration: none;
  //color: black !important;
  text-decoration: none;
  &:hover{
    color: #7368ed !important;
  }

}
.nav-title{
  &:hover{
    color: #7368ed !important;
  }
}
.nuxt-link{
    //color: black;
    text-decoration: none;
    &:hover{
      color: #7368ed !important;
    }
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

</style>



