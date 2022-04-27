<template>
  <div class="full">
    <h2 class="text-md-h4 text-h5 mt-5 mt-md-10 ">{{ title }}</h2>
<!--    <v-divider></v-divider>-->
    <div v-if="$vuetify.breakpoint.mobile" class="model-test-scroller">
      <div v-for="(test, i) in testList" :key="i"  class="card my-2 ml-2">
        <v-card to="/model-test/view/13" class="rounded-lg" width="200" >
          <v-img height="100" :src="test.img"></v-img>
          <div class="text-center py-2 font-weight-bold">
            {{ test.name }}
          </div>
        </v-card>
      </div>
    </div>
    <v-row v-else>
      <v-col v-for="(test, i) in testList" :key="i" cols="12" md="3">
        <v-card to="/model-test/view/13"  class="mx-auto my-3 rounded-lg" max-width="374">
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
  column-gap: 20px;
  -ms-overflow-style: none;
  scrollbar-width: none;
  &::-webkit-scrollbar{
     display: none;
   }
}
</style>
