<template>
  <v-app ref="equationEnabled">
    <v-app-bar :clipped-left="clipped" fixed app flat elevation="1" color="grey lighten-3">
      <v-container class="pa-0 fill-height">
        <NuxtLink class="nav-link-item nav-title-link" to="/admin">
          <v-toolbar-title v-text="title" class="nav-title" />
        </NuxtLink>
        <v-spacer />
        <DesktopNavLinks :links="links"></DesktopNavLinks>
        <v-btn v-if="$vuetify.breakpoint.xsOnly || $vuetify.breakpoint.smOnly " icon @click.stop="rightDrawer = !rightDrawer" >
          <v-icon>mdi-menu</v-icon>
        </v-btn>
      </v-container>
    </v-app-bar>
    <v-main class="mx-0">
      <Nuxt />
    </v-main>

    <v-navigation-drawer v-if="$vuetify.breakpoint.xsOnly || $vuetify.breakpoint.smOnly" v-model="rightDrawer" :right="right" temporary fixed>
      <MobileNavLinks :links="links"></MobileNavLinks>
    </v-navigation-drawer>

    <!-- <v-footer :absolute="!fixed" app color="blue-grey darken-4 white--text" class="py-10">
      <v-row>
        <v-col cols="12" md="5">
          <h1 class="mb-10">Let's work Together</h1>
          <p style="max-width: 50ch;">Our club is open for School, College and University going students all over the country. We work on different olympiads, Scientific Magazine, publishing blogs and many other things. We organize Mega festivals, various Olympiads and competitions. We also organize camps and seminars.</p>
          <p>Please Join us Here if you haven't already.</p>
        </v-col>
        <v-col cols="12" md="3">
          <h1 class="mb-10 text-center">Quick Links</h1>
          <v-row>
            <v-col cols="6" style="">
              <ul>
                <li>List Item 1</li>
              </ul>
            </v-col>
            <v-col cols="6" style="text-align:right;">
              <ul class="" >
                <li>List item 2</li>
              </ul>
            </v-col>
          </v-row>
        </v-col>
        <v-col>
          <h1 class="text-center mb-10">Contact Us</h1>
          <p class="text-center">+880 1633923851</p>
          <p class="text-center">bosonbiggansangho@gmail.com</p>
          <p class="text-center">Leave us a message here.</p>
        </v-col>
      </v-row>
    </v-footer> -->
  </v-app>
</template>

<script>
import DesktopNavLinks from '/components/DesktopNavLinks';
import MobileNavLinks from '/components/MobileNavLinks';

export default {
  auth: true,
  middleware: 'admin',
  components: {MobileNavLinks, DesktopNavLinks},
  transition: 'fade',
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      rightDrawer: false,
      right: false,
      title: 'Admin | ZapQuizy',
      links: [
        {name: 'Model Tests', to: '/admin/model-test/'},
        {name: 'Question Banks', to: '/admin/question-bank/'},
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

.nav-link{
  display: flex;
  padding: 0;
  margin: 0;
  a{
    color: black;
    text-decoration: none;
    &:hover{
      color: blue;
    }
  }
}
.nav-title-link{

  // text-decoration: none;
  color: black !important;
  text-decoration: none;
  &:hover{
    color: blue !important;
  }

}
.nuxt-link{
  color: black;
  text-decoration: none;
  &:hover{
    color: blue !important;
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
