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
            <v-btn color="primary" outlined @click="updateModelTest">
              Update Model Test
            </v-btn>
          </div>
        </v-form>
      </v-card-text>
    </v-card>
    <div class="my-5 mt-10"></div>
<!--    <v-dialog persistent v-model="newQuestionDialog" fullscreen hide-overlay transition="dialog-bottom-transition">-->
<!--      <template class="text-center" v-slot:activator="{ on, attrs }">-->
<!--        <v-btn color="primary" depressed outlined v-bind="attrs" v-on="on">-->
<!--          + Add New Question-->
<!--        </v-btn>-->
<!--      </template>-->
<!--      -->
<!--    </v-dialog>-->
    <v-btn color="primary" v-if="!newQuestionDialog" @click="addQuestionClicked()" depressed outlined >+ Add New Question</v-btn>
    <div v-if="newQuestionDialog" class="question-panel">
      <QuestionPanel

        ref="questionPanel"
        v-on:close-without-save="newQuestionCloseWithoutSave"
        v-on:close-with-save="newQuestionWithSave"
      ></QuestionPanel>
    </div>

    <div class="mt-10">
      <h1 class="text-h4 mb-5 text-center">All Questions</h1>
      <v-list v-for="(question, index) in questions" elevation="3" :key="index" class="mt-8 bordered-list py-0" >
        <div class="text-h6 px-3 py-3" v-html="question.name"></div>
        <div v-for="(option, index2) in question.options" :key="index2" class="option">
          <v-divider></v-divider>
          <v-list-item v-html="option.name" :class="{'correct':option.correct}"></v-list-item>
        </div>
      </v-list>

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
    addQuestionClicked(){
      this.newQuestionDialog = true;
    }

  },
}
</script>

<style scoped>
.correct{
  background-color: #E8F5E9;
}
.bordered-list{
  border: 1px;
}
/*.question-panel{*/
/*  position: absolute;*/
/*  left: 0;*/
/*  top: 0;*/
/*  width: 100vw;*/
/*  height: 100vh;*/
/*}*/
</style>
