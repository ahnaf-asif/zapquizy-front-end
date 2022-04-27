<template>
<div>
  <v-img src="/images/du-iba-analogy.png" alt="image"></v-img>
  <v-card :elevation="$vuetify.breakpoint.mobile?0:1">
    <v-card-title class="text-h5">HSC Physics 1st Paper</v-card-title>
    <v-list>
      <v-list-item class="text-h3"><span class="">৳200</span></v-list-item>
    </v-list>
    <v-list>
      <v-list-item class="font-weight-bold" style="min-height: 55px;">This package includes:</v-list-item>
      <v-list-item style="min-height: 0;"><v-icon>mdi-playlist-check</v-icon> &nbsp; 20 Model Tests</v-list-item>
      <v-list-item style="min-height: 0;"><v-icon>mdi-playlist-check</v-icon> &nbsp; Chapterwise Question Bank</v-list-item>
      <v-list-item style="min-height: 0;"><v-icon>mdi-playlist-check</v-icon> &nbsp; 5340 questions with solutions</v-list-item>
      <v-list-item style="min-height: 0;"><v-icon>mdi-playlist-check</v-icon> &nbsp; Full lifetime access</v-list-item>
    </v-list>
    <div v-if="$auth.user" class="px-2 py-2">
      <v-btn color="primary" depressed block>Buy Package</v-btn>
      <v-btn color="primary" class="mt-2" outlined block>Add To Cart</v-btn>
    </div>
    <div v-else class="px-2 py-2">
      <v-btn color="error" depressed block @click="openLoginForm()">Sign In</v-btn>
    </div>

  </v-card>
  <v-card class="mt-3" :elevation="$vuetify.breakpoint.mobile?0:1">
    <v-card-title class="text-h6">Affiliation Link</v-card-title>
    <v-card-text>
      <v-text-field v-if="$auth.user" v-model="affLink"
        append-outer-icon="mdi-clipboard-check-multiple-outline"
        @click:append-outer="copyAffiliationLink"
        outlined
        dense
        readonly
        persistent-hint
      >
      </v-text-field>
      <v-text-field v-else v-model="dummyAffLink"
        append-outer-icon="mdi-clipboard-check-multiple-outline"
        @click:append-outer="copyAffiliationLink"
        outlined
        dense
        disabled
      >
      </v-text-field>
      <p class="mt-0 pt-0">
        <span>Learn more about affiliation</span> <span @click="openAffiliation" class="popup-link primary--text">here</span>. <br>
        <span v-if="!$auth.user">Please <span @click="openLoginForm()" class="popup-link primary--text">sign in</span> to get your affiliation link.</span>
      </p>
    </v-card-text>
  </v-card>
  <Affiliation ref="affiliation"></Affiliation>
</div>
</template>

<script>
import Affiliation from "./Affiliation";
export default {

  name: "ModelTestPackageBuyCard.vue",
  components: {
    Affiliation
  },
  data(){
    return {
      affLink: 'https://zapquizy.com/model-test/view/12?aff=6539XKSY',
      dummyAffLink: 'Sign in to view affiliation link',
    }
  },
  created(){

  },
  methods: {
    copyAffiliationLink(){
      console.log('hi');
    },
    openLoginForm(){
      this.$nuxt.$emit('open-login-form');
    },
    openAffiliation() {
      this.$refs.affiliation.openAffiliation();
    }
  }
}
</script>

<style scoped>
.popup-link{
  text-decoration: underline;
  cursor: pointer;
}
</style>
