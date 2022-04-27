<template>
  <div>
    <div v-if="!$vuetify.breakpoint.mobile">
      <v-container>
        <v-row >
          <v-col cols="8">
            <ModelTestPackageBuyDetails></ModelTestPackageBuyDetails>
          </v-col>
          <v-col cols="4">
            <ModelTestPackageBuyCard></ModelTestPackageBuyCard>
          </v-col>
        </v-row>
      </v-container>
    </div>
    <div v-else>
      <v-container>
        <ModelTestPackageBuyCard></ModelTestPackageBuyCard>
      </v-container>
      <v-container>
        <ModelTestPackageBuyDetails></ModelTestPackageBuyDetails>
      </v-container>
    </div>
  </div>
</template>

<script>
import ModelTestPackageBuyCard from "../../../components/ModelTestPackageBuyCard";
import ModelTestPackageBuyDetails from "../../../components/ModelTestPackageBuyDetails";

export default {
  transition: 'fade',
  components: {
    ModelTestPackageBuyCard, ModelTestPackageBuyDetails
  },
  data(){
    return {
    }
  },
  mounted(){
    this.$refs.scrollTop = 0;
    if(this.$route.query.aff){
      //console.log('query exists');
    }else {
      //console.log('query does not exist');
    }
  }
}
</script>

<style lang="scss">

</style>
