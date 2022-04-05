<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="6" style="display:flex;align-items:center;">
        <v-img height="300" src="/home-page-image.jpg"></v-img>  
      </v-col>
      <v-col cols="12" md="6" class="homepage">
        <h1 class="text-center mb-5">Boson Biggan Sangho</h1>
        <p>
          Welcome to our official page of Boson Biggan Sangho. We started our journey in 4th July 2014. It is open for School, College and University going students all over the country. We work on different olympiads, Scientific Magazine, publishing blogs and many other things. We organize Mega festivals, various Olympiads and competitions. We also organize camps and seminars.
        </p>
        <p>
          We are affiliated with <a target="_blank" href="https://matholympiad.org.bd/" style="color: blue;text-decoration: none;">Bangladesh Mathematical Olympiad (BdMO)</a>. Bangladesh Mathematical Olympiad committee awarded us as the Best Math Club in 2019.
        </p>
      </v-col>   
    </v-row>

    <h1 class="text-center mt-5">Our Articles</h1>
    <v-divider></v-divider>
    <v-row>
      <v-col v-for="i in 3" :key="i" cols="12" md="4">
        <ArticleComponent></ArticleComponent>
      </v-col>
    </v-row>
    <p class="text-center">
      <v-btn color="primary">View All Articles</v-btn>
    </p>

    <h1 class="text-center mt-10">Blogs</h1>
    <v-divider></v-divider>
    <v-row>
      <v-col v-for="i in 3" :key="i" cols="12" md="4">
        <ArticleComponent></ArticleComponent>
      </v-col>
    </v-row>
    <p class="text-center">
      <v-btn color="error">View All Blogs</v-btn>
    </p>


  </v-container>
</template>

<script>
import ArticleComponent from '@/components/ArticleComponent';

export default {
  components: [
    ArticleComponent
  ],
  data(){
    return {

    }
  },
  mounted(){
    this.$refs.scrollTop = 0;
  }
}
</script>

<style lang="scss">

.homepage{
  p{
    font-size: 1.1em; 
    line-height: 1.6em;
  }
}

</style>