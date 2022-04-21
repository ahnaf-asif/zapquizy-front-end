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
      <v-app-bar v-if="!($vuetify.breakpoint.xsOnly || $vuetify.breakpoint.smOnly)" :clipped-left="clipped" app fixed flat elevation="0" color="white" >
          <v-container class="pa-0  fill-height">

            <NuxtLink class="nav-link-item nav-title-link " to="/">
              <v-toolbar-title v-text="'ZapQuizy'" class="font-weight-bold nav-title text-h4 " />
            </NuxtLink>
            <v-spacer />
            <DesktopNavLinks :links="links"></DesktopNavLinks>
          </v-container>
      </v-app-bar>
    <v-main>
        <Nuxt />
    </v-main>
<!--    {{ $route.path }}-->
    <div v-if="$vuetify.breakpoint.xsOnly || $vuetify.breakpoint.smOnly" class="decoy" style="height: 60px;width: 100%;background: white;">

        <v-card elevation="1" rounded="0" class="bottom-nav">
          <v-btn  icon @click.stop="rightDrawer = !rightDrawer" >
            <v-icon>mdi-menu</v-icon>
          </v-btn>
          <NuxtLink to="/live-exam/"><v-icon :color="$route.path==='/live-exam/'?'primary': 'rgba(0, 0, 0, 0.54)'">mdi-access-point-plus</v-icon></NuxtLink>
          <NuxtLink  to="/"><v-icon :color="$route.path==='/'?'primary': 'rgba(0, 0, 0, 0.54)'">mdi-home-variant-outline</v-icon></NuxtLink>
          <NuxtLink to="/model-test/"><v-icon :color="$route.path==='/model-test/'?'primary': 'rgba(0, 0, 0, 0.54)'">mdi-book-open-outline</v-icon></NuxtLink>
          <NuxtLink to="/profile/"><v-icon :color="$route.path==='/profile/'?'primary': 'rgba(0, 0, 0, 0.54)'">mdi-account-outline</v-icon></NuxtLink>
        </v-card>
    </div>

    <v-navigation-drawer v-if="$vuetify.breakpoint.xsOnly || $vuetify.breakpoint.smOnly" v-model="rightDrawer" :right="right" temporary fixed>
      <MobileNavLinks :links="links"></MobileNavLinks>
    </v-navigation-drawer>

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

export default {
  auth: false,
  components: {MobileNavLinks, DesktopNavLinks},
  transition: 'fade',
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      rightDrawer: false,
      right: false,
      title: '⚡ ZapQuizy',
      links: [
        {name: 'Model Test', to: '/model-test/'},
        {name: 'Quiz', to: '/quiz/'},
        {name: 'Features', to: '/features/'}
      ],
    }
  },
  methods: {

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
}
</script>


<style lang="scss">
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
    color: black;
    text-decoration: none;
    &:hover{
      color: #7368ed;
    }
  }
}
.nav-title-link{

  // text-decoration: none;
  color: black !important;
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
    color: black;
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



