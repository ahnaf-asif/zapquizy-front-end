<template>
  <div>
    <NuxtLink class="navlink" to="/">
      <h1 class="mt-2 text-h3 text-center">Zapquizy</h1>
    </NuxtLink>
    <v-divider class="my-4"></v-divider>
    <v-list>
      <div class="hudai" v-for="link in links" :key="link.name">
        <v-list-item >
          <NuxtLink class="navlink" :to="link.to">{{ link.name }}</NuxtLink>
        </v-list-item>
        <v-divider></v-divider>
      </div>
      <v-list-item v-if="!$auth.user">
        <NuxtLink class="navlink text-uppercase" to="/login/">Login</NuxtLink>
      </v-list-item>
      <v-divider v-if="!$auth.user"></v-divider>
      <v-list-item v-if="!$auth.user">
        <NuxtLink  class="navlink text-uppercase" to="/register/">Register</NuxtLink>
      </v-list-item>
      <v-divider v-if="!$auth.user"></v-divider>
      <v-list-item v-if="$auth.user">
        <NavProfileSection :margin-left="false"></NavProfileSection>
      </v-list-item>
      <v-divider></v-divider>
    </v-list>
  </div>
</template>

<script>
import NavProfileSection from './NavProfileSection';
export default {
  name: "MobileNavLinks.vue",
  props: ['links'],
  components: {
    NavProfileSection
  },
}
</script>

<style lang="scss" scoped>
.navlink{
  color: black;
  text-decoration: none;
  &:hover{
    color: blue;
  }
}
</style>
