<template>
  <div class="white">
    <v-container  class="home-top d-flex justify-space-around">
      <v-row>
        <v-col cols="12" md="6" style="align-items: center; display:flex;justify-content:center;">
          <div class="mt-md-8 home-view" style="display:flex;justify-content: center; " >
            <div class="inside">
              <h1 class="mt-10 text-md-h2 text-h4 py-2 mb-10 black--text font-weight-bold text-center text-md-left">Ace Exams with <span class="primary--text">ZapQuizy!</span></h1>
              <p class="black--text mb-10 text-center text-md-left" style="font-size: 18px;">Prepare for your exams and find and create quizzes for your students. Be a part of our wonderful community!</p>
              <div v-if="!($vuetify.breakpoint.xsOnly || $vuetify.breakpoint.smOnly)" class="buttons ">
                <NuxtLink class="nuxt-link" to="/model-test/" >
                  <v-btn  depressed  large color="primary" class="mt-3">
                    Visit Model Tests
                  </v-btn>
                </NuxtLink>
                &nbsp;&nbsp;
                <NuxtLink class="nuxt-link" to="/quiz/" >
                  <v-btn  depressed outlined color="primary" large style="" class="mt-3">
                    Make a Quiz
                  </v-btn>
                </NuxtLink>
              </div>
              <div v-else style="display: flex;justify-content: center;">
                <div class="d-flex justify-center align-center flex-column">
                  <NuxtLink class="nuxt-link" to="/model-test/" >
                    <v-btn  depressed  large color="primary" class="mt-3" style="width: 200px;">
                      Visit Model Tests
                    </v-btn>
                  </NuxtLink>
                  <NuxtLink class="nuxt-link" to="/quiz/">
                    <v-btn  depressed outlined color="primary" large class="mt-3"  style="width: 200px;">
                      Make a Quiz
                    </v-btn>
                  </NuxtLink>
                </div>
              </div>
            </div>
          </div>
        </v-col>
        <v-col cols="6" v-if="!($vuetify.breakpoint.xsOnly || $vuetify.breakpoint.smOnly)"  style="display:flex; justify-content: flex-end; align-items: center;">
          <v-card elevation="16" width="450" ref="enableEquation" class="white--text" style="background-color: #3a394d;">
            <v-card-title style="font-size: 15px;">What is the biggest mammal in the world?</v-card-title>
            <v-divider></v-divider>
            <v-list class="white--text" style="background-color: #494766;font-size: 15px;">
              <v-list-item v-for="i in 4" :key="i" class="">
                <v-list-item-action>
                  <v-radio-group>
                    <v-radio @click="" class="radio-btn"></v-radio>
                  </v-radio-group>
                </v-list-item-action>
                <v-list-item-content style="font-size: 15px;" class="inside-question white--text">
                  African Elephant
                </v-list-item-content>
<!--                <v-btn color="accent" block>African Elephant</v-btn>-->
              </v-list-item>

            </v-list>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
    <v-container class="mt-10">
<!--      <h1 class="text-center text-h4 font-weight-bold white-text mb-10">Features</h1>-->
      <v-row v-if="!($vuetify.breakpoint.xsOnly || $vuetify.breakpoint.smOnly)">
        <v-col v-for="(feature,i) in features" :key="i" cols="12" md="4" style="row-gap: 40px;">
          <div class="img text-center mb-3">
            <img :src="feature.svg" alt="" style="width: 100px;">
          </div>
          <h3 class="text-center">{{ feature.title }}</h3>
          <p class="text-center">{{feature.description}}</p>
        </v-col>
      </v-row>
      <div class="d-flex justify-space-around" v-else>
        <div class="inside" v-for="(feature,i) in features" :key="i" >
            <div class="left img text-center">
              <img :src="feature.svg" alt="" style="width: 50px;">
            </div>
            <div class="right">
              <p style="line-height: 15px; font-size: 13px; width: 10ch; font-weight: bold;" class="text-center">{{ feature.title }}</p>
            </div>
        </div>
      </div>
    </v-container>
    <v-container fluid class="chepta-model-tests mt-md-10 ">
      <div class="inside">
        <v-container>
          <h1 class="text-center text-md-h4 text-h6 font-weight-bold white-text">All Packages</h1>
          <p class="text-center white-text">Prepare for your examinations</p>
          <v-row class="mt-5">
            <v-col v-for="(pkg,i) in packages" :key="i" cols="12" md="6" class="mobile-col">
              <v-hover v-slot="{ hover }">
                <v-card :elevation="hover ? 16 : 1" class="flat-card-home mx-2" color="#f0f0f0" dark>
                  <v-row class="">
                    <v-col cols="5" class="py-0">
                      <v-img class="flat-card-home" :src="pkg.img"></v-img>
                    </v-col>
                    <v-col cols="7" class="py-0 d-flex align-center">
                      <div >
                        <h1
                          class="text-h6 card-mobile-title text--black px-0"
                          style="color: black !important;"
                          v-text="pkg.title"
                        ></h1>
                        <h5 style="color:#8a8a8a;" class="text--black px-0">
                          {{pkg.subtitle}}
                        </h5>
                      </div>
                    </v-col>
                  </v-row>
                </v-card>
              </v-hover>
            </v-col>
          </v-row>
        </v-container>
      </div>
    </v-container>
    <v-container>
      <h1 class="mt-5 text-center text-md-h4 text-h6 font-weight-bold white-text">Popular Model Test Packages</h1>
      <p class="text-center">Find and practice what you need</p>
      <ModelTestList :test-list="popularModelTests" title=""></ModelTestList>
    </v-container>
  </div>
</template>

<script>
import ModelTestList from "../components/ModelTestList";
export default {
  auth: false,
  transition: 'fade',
  components: {
    ModelTestList
  },
  data(){
    return {
      features: [
        {
          title: 'Join Live Exams',
          description: 'This is the short description I was talking about ',
          svg: '/svgs/live-color.svg'
        },
        {
          title: 'Practice Anywhere',
          description: 'This is the short description I was talking about ',
          svg: '/svgs/practice-color.svg'
        },
        {
          title: 'Climb the Leaderboard',
          description: 'This is the short description I was talking about ',
          svg: '/svgs/leaderboard-lightgreen.svg'
        },
      ],
      packages: [
        {
          title: 'Model Tests',
          subtitle: "345 model tests",
          img: '/ssc-preparation.png'
        },
        {
          title: 'Quizzes',
          subtitle: "Prepare and participate in quizzes",
          img: '/hsc-preparation.png'
        },
        {
          title: 'Courses',
          subtitle: "Take courses and prepare for exams",
          img: '/hsc-preparation.png'
        },
        {
          title: 'Something Else',
          subtitle: "Take courses and prepare for exams",
          img: '/hsc-preparation.png'
        },
      ],
      popularModelTests: [
        {name: 'HSC Botany', img: '/images/hsc-botany.png'},
        {name: 'DU IBA Mathematics', img: '/images/du-iba-mathematics.png'},
        {name: 'SSC Chemistry', img: '/images/ssc-chemistry.png'},
        {name: 'DU IBA Analogy', img: '/images/du-iba-analogy.png'},
      ],
    }
  },
  mounted(){
    this.$refs.scrollTop = 0;
  },
  created(){
    if(process.browser){
      window.MathJax.Hub.Queue([
        "Typeset",
        window.MathJax.Hub,
        this.$refs.enableEquation,
      ]);
    }
  }
};

</script>

<style lang="scss">
.v-radio .v-icon {
  color: white !important;
}

.inside-question{
  p{
    padding: 0;
    margin: 0;
  }
}

@media (min-width: 760px){
  .home-view{
    //min-height: 50vh;
  }
  .flat-card-home{
    height: 120px;cursor:pointer;
  }


}
@media (min-width: 1000px){
  .home-top{
    min-height: 80vh;
  }
}
@media (max-width: 759px){
  .flat-card-home{
    height: 80px;cursor:pointer;
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


</style>
