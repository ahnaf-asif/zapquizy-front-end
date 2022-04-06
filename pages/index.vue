<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="6" style="display:flex;align-items:center;">
        <div class="inside mt-10 mt-md-0">
          <h1 class="text-md-h2 text-h4 mb-5">Zap the quizzes with <span class="text-bold">ZapQuizy</span>!</h1>
          <p>Be a part of our community and start your personalized learning journey!</p>
          <div class="buttons mt-10">
            <NuxtLink class="nuxt-link" to="/model-tests/" >
              <v-btn  depressed color="primary" large style="width: 220px;" class="mt-3">
                Visit Model Tests
              </v-btn>
            </NuxtLink>
            <NuxtLink class="nuxt-link" to="/quiz/" >
              <v-btn  depressed outlined color="primary" large style="width: 220px;" class="mt-3">
                Make a Quiz
              </v-btn>
            </NuxtLink>
          </div>
        </div>
      </v-col>
      <v-col v-if="!($vuetify.breakpoint.xsOnly || $vuetify.breakpoint.smOnly)" cols="12" md="6">
        <img src="/home-page-img.jpg" height="450" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>

export default {
  transition: 'fade',
  components: {
  },
  data(){
    return {

    }
  },
  mounted(){
    this.$refs.scrollTop = 0;
  }
};

</script>

<style lang="scss">

</style>