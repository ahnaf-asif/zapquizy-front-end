<template>
  <v-menu v-if="$auth.user" offset-y rounded="0" content-class="elevation-2" class="navbar-v-menu">
    <template v-slot:activator="{ on, attrs }"  >
      <a class="nav-link-item text-uppercase" :class="{'ml-7':marginLeft}" v-bind="attrs" v-on="on" style="display: flex;">
        {{ $auth.user.name }} <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" role="img" class="v-icon__svg"><path d="M7.41,8.58L12,13.17L16.59,8.58L18,10L12,16L6,10L7.41,8.58Z"></path></svg>
      </a>
    </template>
    <v-list >
      <v-list-item>
        <NuxtLink class="nav-link-item text-uppercase" to="/profile/">Profile</NuxtLink>
      </v-list-item>
      <v-list-item v-if="$auth.user.role === 'admin'">
        <NuxtLink class="nav-link-item text-uppercase" to="/admin/">Admin</NuxtLink>
      </v-list-item>
      <v-list-item>
        <v-btn
          @click="logout"
          color="primary"
          outlined
          depressed
        >
          Log out
        </v-btn>
      </v-list-item>
    </v-list>
  </v-menu>
</template>

<script>
export default {
  name: "NavProfileSection.vue",
  props: ['marginLeft'],
  methods: {
    async logout(){
      await this.$auth.logout();
      await this.$router.push('/');
    }
  },
}
</script>

<style lang="scss" scoped>
.nav-link-item{
  color: black;
  text-decoration: none;
  text-transform: none !important;
  //font-weight: semibold;
  &:hover{
    color: blue;
  }
}
</style>
