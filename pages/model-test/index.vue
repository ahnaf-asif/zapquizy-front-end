<template>
  <v-container>
    <h1 class="mt-5 text-center text-md-h2 text-h4 ">Model Tests</h1>
    <h2 class="text-md-h4 text-h5 mt-5 mt-md-10">SSC</h2>
    <div v-if="$vuetify.breakpoint.mobile" class="model-test-scroller">
      <div v-for="i in 8" :key="i"  class="card">
        <v-hover v-slot="{ hover }">
          <v-card :elevation="hover?16:3" class="mx-auto my-3 rounded-xl ml-2" width="200">
            <v-img height="100" src="/ssc-chemistry.jpg"></v-img>
            <v-card-title class="text-center">
              <div class="text-center">
                SSC Chemistry
              </div>
            </v-card-title>
          </v-card>
        </v-hover>
      </div>
    </div>
    <!-- Desktop SSC modules  -->
    <v-row v-else>
        <v-col v-for="i in 8" :key="i" cols="12" md="3">
            <v-card class="mx-auto my-3 rounded-lg" max-width="374">
                <v-img height="150" src="/ssc-chemistry.jpg"></v-img>
                <v-card-title>SSC Chemistry</v-card-title>
            </v-card>
        </v-col>
    </v-row>
    <h2 class="text-md-h4 text-h5 mt-5 mt-md-10">HSC</h2>
    <div v-if="$vuetify.breakpoint.mobile" class="model-test-scroller">
      <div v-for="i in 8" :key="i"  class="card">
        <v-hover v-slot="{ hover }">
          <v-card :elevation="hover?16:3" class="mx-auto my-3 rounded-xl ml-2" width="200">
            <v-img height="100" src="/hsc-botany.jpg"></v-img>
            <v-card-title class="text-center">
              <div class="text-center">
                HSC Botany
              </div>
            </v-card-title>
          </v-card>
        </v-hover>
      </div>
    </div>
    <v-row v-else>
        <v-col v-for="i in 8" :key="i" cols="12" md="3">
          <v-card class="mx-auto my-3 rounded-lg" max-width="374">
            <v-img height="150" src="/hsc-botany.jpg"></v-img>
            <v-card-title>HSC Botany</v-card-title>
          </v-card>
        </v-col>
    </v-row>
  </v-container>
</template>

<script>

export default {
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
}
</script>

<style lang="scss">
.model-test-scroller{
  overflow-x: auto;
  display: flex;
  column-gap: 20px;
  -ms-overflow-style: none;
  scrollbar-width: none;
  &::-webkit-scrollbar{
    display: none;
  }
}
</style>
