<template>
  <v-container>
    <h1 class="mt-5 text-center text-md-h2 text-h4 mb-10">Model Tests</h1>

    <h2 class="text-md-h4 text-h5">SSC</h2>
    <v-divider></v-divider>
    <v-row>
        <v-col v-for="i in 3" :key="i" cols="12" md="4">
            <v-card class="mx-auto my-3" max-width="374">
                <v-img height="200" src="/ssc-chemistry.jpg"></v-img>
                <v-card-title>SSC Botany</v-card-title>

                <v-card-text>
                    <div class="my-2 text-subtitle-1">
                        ৳ 200
                    </div>
                    <p>Lorem ipsum, dolor sit amet consectetur adipisicing elit. Quis odit similique eius qui, maiores magnam ex velit nesciunt non aspernatur eligendi quod</p>
                </v-card-text>
                <v-divider class="mx-4"></v-divider>
                <v-card-actions class="mt-3 text-center">
                    <v-btn text color="primary">View Details</v-btn>
                </v-card-actions>
            </v-card>
        </v-col>
    </v-row>
    <h2 class="text-md-h4 text-h5 mt-10">HSC</h2>
    <v-divider></v-divider>
    <v-row>
        <v-col v-for="i in 3" :key="i" cols="12" md="4">
            <v-card class="mx-auto my-3" max-width="374">
                <v-img height="200" src="/hsc-botany.jpg"></v-img>
                <v-card-title>HSC Botany</v-card-title>

                <v-card-text>
                    <div class="my-2 text-subtitle-1">
                        ৳ 200
                    </div>
                    <p>Lorem ipsum, dolor sit amet consectetur adipisicing elit. Quis odit similique eius qui, maiores magnam ex velit nesciunt non aspernatur eligendi quod</p>
                </v-card-text>
                <v-divider class="mx-4"></v-divider>
                <v-card-actions class="mt-3 text-center">
                    <v-btn text color="primary">View Details</v-btn>
                </v-card-actions>
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

</style>
