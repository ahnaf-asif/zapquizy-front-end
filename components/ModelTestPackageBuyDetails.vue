<template>
  <v-container>
<!--    <h3>All Model Tests:</h3>-->

    <h1 class="text-h5 text-bold mt-5 mb-3">Model Tests:</h1>

    <v-expansion-panels elevation="0">
      <v-expansion-panel v-for="i in modelTestsShow" :key="i">
        <v-expansion-panel-header>
          <div>
            <v-icon>mdi-lock</v-icon>  SSC Physics Model Test {{i}}
          </div>
        </v-expansion-panel-header>
        <v-expansion-panel-content>
          <span class="secondary--text">
            Please buy the model test package to access the model test
          </span>
        </v-expansion-panel-content>
      </v-expansion-panel>
    </v-expansion-panels>
    <v-btn class="mt-5" @click="ModelTestsShowToggle" depressed color="secondary" outlined block>Load All Model Tests</v-btn>
    <h1 class="text-h5 text-bold mt-10 mb-3">Question Banks:</h1>
    <v-expansion-panels elevation="0">
      <v-expansion-panel v-for="i in 5" :key="i">
        <v-expansion-panel-header>
          <div>
            <v-icon>mdi-lock</v-icon>  Chapter {{i}} : পরিবেশ বিজ্ঞান
          </div>
        </v-expansion-panel-header>
        <v-expansion-panel-content>
          <span class="secondary--text">
            Please buy the model test package to access the Question Bank
          </span>
        </v-expansion-panel-content>
      </v-expansion-panel>
    </v-expansion-panels>
    <v-btn class="mt-5" depressed color="secondary" outlined block>Load All Question Banks</v-btn>

  </v-container>
</template>

<script>
export default {
  name: "ModelTestPackageBuyDetails.vue",
  data(){
    return {
      modelTestsShow: 5,
      questionBanksShow: 5,
    }
  },
  methods: {
    ModelTestsShowToggle(){
      this.modelTestsShow = 20;
    }
  }
}
</script>

<style scoped>
.v-expansion-panel::before {
  box-shadow: none;
}
</style>
