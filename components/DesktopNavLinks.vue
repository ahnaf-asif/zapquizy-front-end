<template>
  <div style="display:flex;align-items: center;" class="nav-link" v-if="!($vuetify.breakpoint.xsOnly || $vuetify.breakpoint.smOnly)" >

      <NuxtLink v-for="link in links" :key="link.name" class="ml-7 nav-link-item text-uppercase" :to="link.to">{{ link.name }}</NuxtLink>

      <NuxtLink v-if="!$auth.user" class="ml-7 nav-link-item primary-text-dark" to="/login/">
        <v-btn outlined color="indigo darken-4" class="primary-text-dark"> Login &nbsp; <v-icon>mdi-arrow-right-bold-box-outline</v-icon></v-btn>
      </NuxtLink>

<!--      <NuxtLink v-if="!$auth.user" class="ml-7 nav-link-item text-uppercase" to="/register/">Register</NuxtLink>-->

      <NavProfileSection :margin-left="true"></NavProfileSection>

  </div>

</template>

<script>
import NavProfileSection from "./NavProfileSection";
export default {
  name: "DesktopNavLinks",
  props: ['links'],
  components: {
    NavProfileSection
  },
  data(){
    return {

    }
  },

}
</script>

<style lang="scss" scoped>
.nav-link-item{
  color: black;
  text-decoration: none;
  text-transform: none !important;
  //font-weight: bold;
  &:hover{
    color: blue;
  }
}
</style>
