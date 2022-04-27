<template>
  <div style="display:flex;align-items: center;" class="nav-link" v-if="!($vuetify.breakpoint.xsOnly || $vuetify.breakpoint.smOnly)" >

      <NuxtLink v-for="link in links" :key="link.name" class="link--text ml-7 nav-link-item text-uppercase" :to="link.to">{{ link.name }}</NuxtLink>

      <NuxtLink v-if="!$auth.user" class="ml-7 nav-link-item link--text" to="/login/">
        <v-btn outlined color="primary"> Login &nbsp; <v-icon>mdi-arrow-right-bold-box-outline</v-icon></v-btn>
      </NuxtLink>

      <NavProfileSection v-if="!$vuetify.breakpoint.mobile" :margin-left="true"></NavProfileSection>
      <v-switch v-model="$vuetify.theme.dark"></v-switch>

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
  text-decoration: none;
  text-transform: none !important;
  &:hover{
    color: #7368ed;
  }
}
</style>
