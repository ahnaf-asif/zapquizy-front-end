<template>
  <div class="nav-link" v-if="!($vuetify.breakpoint.xsOnly || $vuetify.breakpoint.smOnly)" >

      <NuxtLink v-for="link in links" :key="link.name" class="ml-7 nav-link-item text-uppercase" :to="link.to">{{ link.name }}</NuxtLink>

      <NuxtLink v-if="!$auth.user" class="ml-7 nav-link-item text-uppercase" to="/login/">Login</NuxtLink>

      <NuxtLink v-if="!$auth.user" class="ml-7 nav-link-item text-uppercase" to="/register/">Register</NuxtLink>

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
  &:hover{
    color: blue;
  }
}
</style>
