<template>
  <v-container>
    <h1 class="text-h4 my-5 text-center ">{{ modelTest.name }}</h1>
    <v-card elevation="1">
      <v-card-text>
        <v-form ref="newModelTestForm" v-model="modelTestForm" :valid="isModelTestFormValid">
          <v-text-field
            v-model="modelTest.name"
            label="Name"
            :rules="[v => !!v || 'Name is required']"
            required></v-text-field>
          <v-text-field
            v-model="modelTest.duration"
            label="Duration(minutes)"
            type="number"
            :rules="[v => !!v || 'Duration is required']"
            required></v-text-field>
          <v-select
            v-model="modelTest.level_id"
            :items="levels"
            item-value="id"
            item-text="name"
            :rules="[v => !!v || 'Level is required']"
            label="Level"
            required >
          </v-select>
          <v-select
            v-model="modelTest.subject_id"
            :items="subjects"
            item-value="id"
            item-text="name"
            :rules="[v => !!v || 'Subject is required']"
            label="Subject"
            required >
          </v-select>
          <div class="text-center">
            <v-btn color="primary"  @click="updateModelTest">
              Update Model Test
            </v-btn>
          </div>
        </v-form>
      </v-card-text>
    </v-card>
    <div class="my-5 mt-10"></div>
    <v-dialog persistent v-model="newQuestionDialog" fullscreen hide-overlay transition="dialog-bottom-transition">
      <template class="text-center" v-slot:activator="{ on, attrs }">
        <v-btn color="primary" depressed outlined v-bind="attrs" v-on="on">
          + Add New Question
        </v-btn>
      </template>
      <QuestionPanel
        ref="questionPanel"
        v-on:close-without-save="newQuestionCloseWithoutSave"
        v-on:close-with-save="newQuestionWithSave"
      ></QuestionPanel>
    </v-dialog>
    <div class="mt-10">
      <h1 class="text-h4 text-center">All Questions</h1>
<!--      <v-divider></v-divider>-->
    </div>

  </v-container>
</template>

<script>

import QuestionPanel from "../../../components/QuestionPanel";

export default {
  name: "ModelTestInsideVue",
  auth: true,
  middleware: 'admin',
  layout: 'admin',
  transition: 'fade',

  components: {
    QuestionPanel
  },
  data(){
    return {
      newQuestionDialog: false,
      modelTestForm: '',
      isModelTestFormValid: true,
    }
  },
  created(){
    // console.log(this.questions);
  },
  async asyncData({ $http, params }){
    let subjects = await $http.$get('/api/subjects');
    let levels = await $http.$get('/api/levels');
    let modelTest = await $http.$get(`/api/admin/model-test/get/${params.id}`);
    let questions = await $http.$get(`/api/admin/model-test/${params.id}/questions/get`);
    return { subjects, levels, modelTest, questions };
  },
  methods: {
    async updateModelTest(){
      try{
        let res = await this.$axios.post(`/api/admin/model-test/update/${this.modelTest.id}`, this.modelTest);
        this.modelTest = res.data;
      }catch(er){
        console.log(er);
      }
    },
    newQuestionCloseWithoutSave(){
      this.newQuestionDialog = false;
    },
    async newQuestionWithSave(question){
      console.log(question);
      try{
        let res = await this.$axios.post(`/api/admin/model-test/${this.modelTest.id}/questions/add`, question);
        this.questions = res.data;
        this.$refs.questionPanel.clearQuestions();
        this.newQuestionDialog = false;
      }catch(er){
        console.log(er);
      }
    },

  },
}
</script>

<style scoped>

</style>
