<template>
<v-container class="d-flex justify-center align-items-center flex-column" fill-height style="">
  <img src="/svgs/live-color.svg" alt="live" width="150">

  <div class="py-5 px-3">
    <h1 class="text-h3 text-center">Join Live Quiz</h1>
    <h1 class="text-h5 primary--text text-center mt-15">Enter Exam Code</h1>
    <div class="otp mt-2" style="width: 300px;">
        <v-otp-input color="primary" length="4"></v-otp-input>
        <div v-if="0===1" class="error-message">
          <p class="error--text text-center" style="font-size: 12px;">No Live Exams Found with that code</p>
        </div>
        <div class="d-flex justify-center">
          <v-btn to="/quiz/live/XXXX" class="mt-15" color="primary" x-large>Proceed</v-btn>
        </div>
    </div>
  </div>
</v-container>
</template>

<script>
export default {
  name: "index.vue",
  transition: 'fade',
}
</script>

<style scoped>

</style>
