<template>
  <v-list class="nav-link" v-if="!($vuetify.breakpoint.xsOnly || $vuetify.breakpoint.smOnly)" >
    <v-list-item>

      <v-menu offset-y rounded="0" content-class="elevation-2" class="navbar-v-menu">
        <template v-slot:activator="{ on, attrs }"  >
          <a class="nav-link-item text-uppercase" v-bind="attrs" v-on="on" style="display: flex;">
            Archive <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" role="img" class="v-icon__svg"><path d="M7.41,8.58L12,13.17L16.59,8.58L18,10L12,16L6,10L7.41,8.58Z"></path></svg>
          </a>
        </template>
        <v-list >
          <v-list-item>
            <NuxtLink class="nav-link-item text-uppercase" to="/blogs/">Blogs</NuxtLink>
          </v-list-item>
          <v-list-item>
            <NuxtLink class="nav-link-item text-uppercase" to="/articles/">Articles</NuxtLink>
          </v-list-item>
          <v-list-item>
            <NuxtLink class="nav-link-item text-uppercase" to="/resources/">Resources</NuxtLink>
          </v-list-item>
          <v-list-item>
            <NuxtLink class="nav-link-item text-uppercase" to="/faq/">FAQ</NuxtLink>
          </v-list-item>
        </v-list>
      </v-menu>
    </v-list-item>
    <v-list-item>
      <NuxtLink class="nav-link-item text-uppercase" to="/problems/">Problems</NuxtLink>
    </v-list-item>
    <v-list-item>
      <NuxtLink class="nav-link-item text-uppercase" to="/gallery/">Gallery</NuxtLink>
    </v-list-item>
    <v-list-item>
      <NuxtLink class="nav-link-item text-uppercase" to="/about/">About</NuxtLink>
    </v-list-item>
    <v-list-item>
      <NuxtLink class="nav-link-item text-uppercase" to="/contact/">Contact</NuxtLink>
    </v-list-item>
    <v-list-item>
      <NuxtLink class="nav-link-item text-uppercase" to="/login/">Login</NuxtLink>
    </v-list-item>
    <v-list-item>
      <NuxtLink class="nav-link-item text-uppercase" to="/register/">Register</NuxtLink>
    </v-list-item>
  </v-list>

</template>

<script>
export default {
name: "DesktopNavLinks"
}
</script>

<style scoped>

</style>
