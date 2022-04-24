<template>
  <v-container>
    <h1 class="my-5 text-h4 text-center">Add New Model Test</h1>
    <v-card elevation="1">
      <v-card-text>
        <v-form ref="newModelTestForm" v-model="newModelTestForm" :valid="isNewModelTestFormValid">
          <v-text-field
            v-model="newModelTestName"
            label="Name"
            :rules="[v => !!v || 'Name is required']"
            required></v-text-field>
          <v-text-field
            v-model="newModelTestDuration"
            label="Duration(minutes)"
            type="number"
            :rules="[v => !!v || 'Duration is required']"
            required></v-text-field>
          <v-select
            v-model="selectedModelTestPackage"
            :items="modelTestPackages"
            item-text="name"
            item-value="id"
            :rules="[v => !!v || 'Model Test Package is required']"
            label="Model Test Package"
            required >
          </v-select>
          <v-select
            v-model="selectedLevel"
            :items="levels"
            item-text="name"
            item-value="id"
            :rules="[v => !!v || 'Level is required']"
            label="Level"
            required >
          </v-select>
          <v-select
            v-model="selectedSubject"
            :items="subjects"
            item-text="name"
            item-value="id"
            :rules="[v => !!v || 'Subject is required']"
            label="Subject"
            required >
          </v-select>
          <div class="text-center">
            <v-btn color="primary" large @click="addModelTest">
              Add Model Test
            </v-btn>
          </div>
        </v-form>
      </v-card-text>
    </v-card>
    <h1 class="mt-10 mb-5 text-h4 text-center">All Model Tests</h1>

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
    <v-snackbar v-model="snackbar" :timeout="3000" bottom right color="success">
      {{ snackbarText }}
      <template v-slot:action="{ attrs }">
        <v-btn color="white" text v-bind="attrs" @click="snackbar = false">
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </v-container>
</template>

<script>

export default {
  name: "index.vue",
  auth: true,
  middleware: 'admin',
  layout: 'admin',
  transition: 'fade',
  data(){
    return {
      newModelTestName: '',
      newModelTestDuration: '',
      newModelTestForm: '',
      isNewModelTestFormValid: true,
      selectedLevel: null,
      selectedModelTestPackage: null,

      selectedSubject: null,

      modelTestHeaders: [
        { text: 'Model Test Name', value: 'name'},
        { text: 'Subject', value: 'subject.name'},
        { text: 'Level', value: 'level.name'},
        { text: 'Action', value: 'enter'},
        { text: 'Action', value: 'delete'}
      ],
      snackbar: false,
      snackbarText: null,
    }
  },
  async asyncData({ $http }){
    let subjects = await $http.$get('/api/subjects');
    let levels = await $http.$get('/api/levels');
    let modelTests = await $http.$get('/api/admin/model-test/get');
    let modelTestPackages = await $http.$get('/api/admin/model-test-package/get');
    return { subjects, levels, modelTests, modelTestPackages };
  },
  methods: {
    async addModelTest(){
      if(this.$refs.newModelTestForm.validate()){
        let newModelTest = {
          name: this.newModelTestName,
          level_id: this.selectedLevel,
          subject_id: this.selectedSubject,
          duration: this.newModelTestDuration,
          model_test_package_id: this.selectedModelTestPackage,
        }
        try{
          this.modelTests = await this.$axios.$post('/api/admin/model-test/add', newModelTest);
          this.snackbar = true;
          this.snackbarText = 'Model Test added';
        }catch(err){
          console.log(err);
        }
      }
    },
    async deleteModelTest(id){
      if(confirm('Do you really want to delete the model test?')){
        try{
          this.modelTests = await this.$axios.$post(`/api/admin/model-test/delete/${id}`);
          this.snackbar = true;
          this.snackbarText = 'Model Test deleted successfully';
        }catch(err){
          console.log(err);
        }
      }
    }
  },
}
</script>

<style lang="scss" scoped>
.btnlink{
  color: white !important;
  text-decoration: none !important;
  &:hover{

  }
}
</style>
