<template>
  <div class="mt-16">
      <div class="mt-md-8 home-view" style="display:flex;justify-content: center; " >
        <div class="inside">
          <h1 class="text-md-h2 text-h3 py-4 mb-5 text-center primary-text-dark">Zap the quizzes with <span class="font-weight-bold">ZapQuizy!</span></h1>
          <p class="text-center" style="font-size: 18px;">Prepare for your exams and find and create quizzes for your students.</p>
          <p class="text-center p-only-desktop" style="font-size: 18px;"> Be a part of our community and start your personalized learning journey!</p>
          <div class="buttons text-center">
            <NuxtLink class="nuxt-link" to="/model-test/" >
              <v-btn  depressed  large style="" class="mt-3 primary-bg-dark">
                Visit Model Tests
              </v-btn>
            </NuxtLink>
            &nbsp;&nbsp;
            <NuxtLink class="nuxt-link" to="/quiz/" >
              <v-btn  depressed outlined color="indigo darken-4" large style="" class="mt-3">
                Make a Quiz
              </v-btn>
            </NuxtLink>
          </div>
        </div>
      </div>

    <v-container fluid class="model-tests grey lighten-4 py-8 mt-16">
      <v-container class="inside">
        <h1 class="text-center text-h4 font-weight-bold">All Model Test Packages</h1>
        <p class="text-center">Prepare for your examinations</p>
        <v-row class="mt-5">
          <v-col v-for="i in 4" :key="i" cols="12" md="6" class="mobile-col">
            <v-hover v-slot="{ hover }">
              <v-card :elevation="hover ? 16 : 0" class="flat-card-home" color="white" dark>
                <v-row class="">
                  <v-col cols="5" class="py-0">
                    <v-img class="flat-card-home" src="/flat-placeholder.jpg"></v-img>
                  </v-col>
                  <v-col cols="7" class="py-0 d-flex align-center">
                    <div >
                      <v-card-title
                        class="text-md-h5 card-mobile-title text--black px-0"
                        style="color: black !important;"
                        v-text="'University Preparation'"
                      ></v-card-title>
                      <v-card-subtitle style="color:#8a8a8a;" class="text--black px-0">
                        <v-icon style="color: #8a8a8a">mdi-account-check-outline</v-icon> 69,000 students enrolled
                      </v-card-subtitle>
                    </div>
                  </v-col>
                </v-row>
              </v-card>
            </v-hover>
          </v-col>
        </v-row>
      </v-container>
    </v-container>
    <v-container  class="popular-model-testspy-8 mt-16">
      <h1 class="text-center text-h4 font-weight-bold mb-10">Popular Model Tests</h1>
      <v-row>
        <v-col v-for="i in 4" :key="i" cols="12" md="3">
          <v-card class="mx-auto my-2" max-width="374">
            <v-img height="150" src="/du-iba-eng.png"></v-img>
            <v-card-title>SSC Botany</v-card-title>

            <v-card-text>
              <div class="text-subtitle-1">
                ৳ 200
              </div>
              <p>Lorem ipsum, dolor sit amet consectetur adipisicing elit.</p>
            </v-card-text>
            <v-divider class="mx-4"></v-divider>
            <v-card-actions class="mt-1 text-center">
              <v-btn text color="primary">View Details</v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
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
@media (min-width: 760px){
  .home-view{
    min-height: 50vh;
  }
  .flat-card-home{
    height: 120px;cursor:pointer;
  }
}
@media (max-width: 759px){
  .flat-card-home{
    height: 90px;cursor:pointer;
  }
  .card-mobile-title{
    font-size: 17px;
    font-weight: bold;
  }
  .mobile-col{
    padding-left: 0;
    padding-right: 0;
  }
  .p-only-desktop{
    display: none;
  }
}
.model-tests{
  width: 100vw !important;
}
</style>
