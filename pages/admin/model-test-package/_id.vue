<template>
  <v-container>
    <h1 class="my-5 text-h4 text-center">{{ modelTestPackage.name }}</h1>
    <v-row>
      <v-col cols="12" md="6">
        <img alt="Cover Image" style="width: 100%;" :src="`${$axios.defaults.baseURL}${modelTestPackage.cover_image}`" />
      </v-col>
      <v-col cols="12" md="6">

        <v-card elevation="1">
          <v-card-text>
            <v-alert v-if="submitError" type="error">{{submitErrorText}}</v-alert>
            <v-form ref="newModelTestForm">
              <v-text-field
                v-model="modelTestPackage.name"
                label="Name"
                :rules="[v => !!v || 'Name is required']"
                hint="Try to set an unique name (not necessary, but recommended)"
                required></v-text-field>
              <v-text-field
                v-model="modelTestPackage.price"
                label="Package Price (in tk)"
                :rules="[v => !!v || 'Price is required']"
                type="number"
                required></v-text-field>
              <v-select
                v-model="modelTestPackage.level_id"
                :items="levels"
                item-text="name"
                item-value="id"
                :rules="[v => !!v || 'Level is required']"

                label="Level"
                required >
              </v-select>
              <v-select
                v-model="modelTestPackage.subject_id"
                :items="subjects"
                item-text="name"
                item-value="id"
                :rules="[v => !!v || 'Subject is required']"
                label="Subject"
                required >
              </v-select>
              <div class="text-center">
                <v-btn color="primary" large @click="addModelTestPackage()">
                  Save
                </v-btn>
              </div>
            </v-form>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <h1 class="mt-10 mb-5 text-h4 text-center">Model Tests</h1>
    <v-data-table
      :headers="modelTestHeaders"
      :items="modelTests"
      :items-per-page="10"
      class="elevation-1"
    >
      <template v-slot:item.enter="{ item }">

        <v-btn small color="success">
          <NuxtLink class="btnlink" :to="`/admin/model-test/${item.id}`">Enter</NuxtLink>
        </v-btn>

      </template>
      <template v-slot:item.delete="{ item }">

        <v-btn @click="deleteModelTest(item.id)" small color="error">
          Delete
        </v-btn>

      </template>
    </v-data-table>
  </v-container>
</template>

<script>
export default {
  name: "modelTestPackageId",
  auth: true,
  middleware: 'admin',
  layout: 'admin',
  transition: 'fade',
  async asyncData({ $http, params }){
    let subjects = await $http.$get('/api/subjects');
    let levels = await $http.$get('/api/levels');
    let modelTestPackage = await $http.$get(`/api/admin/model-test-package/get/${params.id}`);
    let modelTests = await $http.$get(`/api/admin/model-test-package/${params.id}/model-tests`);
    return { subjects, levels, modelTestPackage, modelTests };
  },
  created(){
    console.log(this.modelTests);
  },
  data(){
    return {
      newName: null,
      selectedSubject: null,
      selectedLevel: null,
      coverImage: null,
      price: null,

      submitError: false,
      submitErrorText: '',
      modelTestHeaders: [
        { text: 'Model Test Name', value: 'name'},
        { text: 'Subject', value: 'subject.name'},
        { text: 'Level', value: 'level.name'},
        { text: 'Action', value: 'enter'},
        { text: 'Action', value: 'delete'}
      ],
    }
  },
  methods : {
    async addModelTestPackage(){
      this.submitError = false;
      try{
        const config = {
          headers: {
            'content-type': 'multipart/form-data'
          }
        }
        const data = new FormData();
        data.append('file', this.coverImage);
        const res = await this.$axios.post('/api/image/upload', data, config);
        console.log(res);
        if(res.status === 200){
          let newModelTestPackage = {
            name: this.newName,
            price: this.price,
            subject_id: this.selectedSubject,
            level_id: this.selectedLevel,
            cover_image: res.data.file_path,
          };
          try{
            // console.log(newModelTestPackage);
            const res = await this.$axios.post('/api/admin/model-test-package/add', newModelTestPackage);
            // console.log(res.data);
            this.modelTestPackages = res.data;

          }catch(e){
            console.log(e);
          }
        }
      }catch(e){
        this.submitError = true;
        this.submitErrorText = "Image Upload failed";
      }
    },
    async deleteModelTestPackage(id){

    },
    async deleteModelTest(id){

    }
  }
}
</script>

<style scoped>

</style>
