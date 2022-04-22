<template>
  <v-container>
    <h1 class="mt-5 mb-5 text-center text-md-h2 text-h4 ">Model Tests</h1>
    <ModelTestList :test-list="ssc" title="SSC Preparation"></ModelTestList>
    <ModelTestList :test-list="hsc" title="HSC Preparation"></ModelTestList>
    <ModelTestList :test-list="universityAdmission" title="University Admission"></ModelTestList>
  </v-container>
</template>

<script>
import ModelTestList from "../../components/ModelTestList";
export default {
  transition: 'fade',
  components: {
    ModelTestList
  },
  data(){
    return {
      hsc: [
        {name: 'HSC Botany', img: '/images/hsc-botany.png'},
        {name: 'HSC Zoology', img: '/images/hsc-zoology.png'},
        {name: 'HSC Botany', img: '/images/hsc-botany.png'},
        {name: 'HSC Zoology', img: '/images/hsc-zoology.png'},
      ],
      ssc: [
        {name: 'SSC Chemistry', img: '/images/SSC-chemistry.png'},
        {name: 'SSC Physics', img: '/images/SSC-physics.png'},
        {name: 'SSC Chemistry', img: '/images/SSC-chemistry.png'},
        {name: 'SSC Physics', img: '/images/SSC-physics.png'},
      ],
      universityAdmission: [
        {name: 'DU IBA Mathematics', img: '/images/du-iba-mathematics.png'},
        {name: 'DU IBA Analogy', img: '/images/du-iba-analogy.png'},
        {name: 'DU IBA Mathematics', img: '/images/du-iba-mathematics.png'},
        {name: 'DU IBA Analogy', img: '/images/du-iba-analogy.png'},
      ]
    }
  },
  mounted(){
    this.$refs.scrollTop = 0;
  }
}
</script>

<style lang="scss">

</style>
