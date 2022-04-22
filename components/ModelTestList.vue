<template>
  <div class="full">
    <h2 class="text-md-h4 text-h5 mt-5 mt-md-10 ">{{ title }}</h2>
<!--    <v-divider></v-divider>-->
    <div v-if="$vuetify.breakpoint.mobile" class="model-test-scroller">
      <div v-for="test in testList" :key="test"  class="card">
        <v-hover to="/" v-slot="{ hover }">
          <v-card :elevation="hover?5:3" class="mx-auto my-3 rounded-lg ml-3" width="200">
            <v-img height="100" :src="test.img"></v-img>
            <div class="text-center py-2 font-weight-bold">
              {{ test.name }}
            </div>
          </v-card>
        </v-hover>
      </div>
    </div>
    <v-row v-else>
      <v-col v-for="test in testList" :key="test" cols="12" md="3">
        <v-card class="mx-auto my-3 rounded-lg" max-width="374">
          <v-img height="150" :src="test.img"></v-img>
          <v-card-title>{{ test.name }}</v-card-title>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  name: "ModelTestList",
  props: ['testList', 'title'],
}
</script>

<style lang="scss" scoped>
.model-test-scroller{
  overflow-x: auto;
  display: flex;
  column-gap: 10px;
  -ms-overflow-style: none;
  scrollbar-width: none;
  &::-webkit-scrollbar{
     display: none;
   }
}
</style>
