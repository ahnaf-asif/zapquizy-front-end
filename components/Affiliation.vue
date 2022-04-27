<template>
  <v-dialog v-model="affiliation" max-width="600">
    <v-card>
      <v-toolbar
        elevation="0"
        color="primary white--text text-h5"
      >Affiliation</v-toolbar>
      <v-card-text>
        <h2>hello world</h2>
      </v-card-text>
      <v-card-actions class="justify-end">
        <v-btn text @click="affiliation = false">Close</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: "Affiliation.vue",
  data(){
    return {
      affiliation: false,
    }
  },
  methods: {
    openAffiliation(){
      this.affiliation = true;
    }
  }
}
</script>

<style scoped>

</style>
