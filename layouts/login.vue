<template>
  <v-app ref="equationEnabled">
    <v-main>
      <Nuxt />
    </v-main>
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
.v2-chip{
  position: fixed;
  z-index: 6;
  margin-left: auto;
  margin-right: auto;
  left: 0;
  right: 0;
  text-align: center;
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



