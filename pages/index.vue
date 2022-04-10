<template>
  <v-container fill-height style="display:flex;align-items:center;justify-content: center;">
      <div>
        <div class="inside">
          <h1 class="text-md-h2 text-h3 py-4 mb-5 text-center primary-gradient-text">Zap the quizzes with <span class="font-weight-bold">ZapQuizy!</span></h1>
          <p class="text-center" style="font-size: 18px;">Prepare for your exams and find and create quizzes for your students. <br> Be a part of our community and start your personalized learning journey!</p>
          <div class="buttons text-center">
            <NuxtLink class="nuxt-link" to="/model-test/" >
              <v-btn  depressed  large style="width: 220px;" class="mt-3 primary-bg-dark">
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
      </div>
  </v-container>
</template>

<script>

export default {
  auth: false,
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
