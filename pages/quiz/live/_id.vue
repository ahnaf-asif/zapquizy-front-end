<template>
  <v-container>
    <div class="exam-header mt-15">
      <h1 class="text-h4 text-center">Physics 1st Paper Exam</h1>
      <p class="text-center mt-5">Starts at: 12/05/2023 10:30 Pm<br>50 Minutes</p>
      <div v-if="!$auth.user" class="mt-16">
        <p class="error--text text-center">Please Sign In before entering the exam !</p>
        <div  class="mt-5 d-flex justify-center">
          <v-btn color="error" @click="openLoginForm()" x-large>Sign In</v-btn>
        </div>
      </div>
      <div v-else-if="!quizStarted" class="mt-16">
        <h1 class="text-h1 text-center primary--text">2:35</h1>
        <p style="font-size: 12px;" class="text-center primary--text mt-5">Before starts</p>
      </div>
      <div v-else class="mt-16 d-flex justify-center">
        <v-btn color="primary" x-large>Enter Quiz</v-btn>
      </div>

    </div>
  </v-container>
</template>

<script>
export default {
  name: "index.vue",
  transition: 'fade',
  data(){
    return {
      quizStarted: false,
      quiz: {
        name: 'Physics First Paper Exam',
        created_at: '12/05/2022',
        duration: '10',

      }
    }
  },
  methods: {
    openLoginForm(){
      this.$nuxt.$emit('open-login-form');
    }
  }
}
</script>

<style scoped>

</style>
