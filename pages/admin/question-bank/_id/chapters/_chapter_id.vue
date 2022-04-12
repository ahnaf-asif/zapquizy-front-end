<template>
  <v-container>
    <NuxtLink class="nuxtbtn" :to="`/admin/question-bank/${question_bank_id}`">
      <v-btn small class="primary-bg-dark ">back to the Question Bank</v-btn>
    </NuxtLink>
    <h1 class="text-h4 my-5 text-center ">{{ chapter.name }}</h1>
    <v-card elevation="1">
      <v-card-text>
        <v-form ref="newchapterForm" v-model="chapterForm" :valid="isChapterFormValid">
          <v-text-field
            v-model="chapter.name"
            label="Name"
            :rules="[v => !!v || 'Name is required']"
            required>
          </v-text-field>
          <div class="text-center">
            <v-btn color="primary" outlined @click="updatechapter">
              Update Chapter
            </v-btn>
          </div>
        </v-form>
      </v-card-text>
    </v-card>
    <div class="my-5 mt-10"></div>
    <div ref="editor" class="question-panel">
      <QuestionPanel

        ref="questionPanel"
        v-on:close-without-save="newQuestionCloseWithoutSave"
        v-on:close-with-save="newQuestionWithSave"
        v-on:save-edited-question="saveEditedQuestion"
      ></QuestionPanel>
    </div>

    <div :key="componentKey" class="mt-10" ref="equations">
      <h1 class="text-h4 mb-5 text-center">All Questions</h1>
      <v-list v-for="(question, index) in questions" elevation="3" :key="index" class="mt-8 bordered-list py-0" >
        <div class="text-h6 px-3 py-3 d-flex flex-row justify-space-between">
          <div style="text-underline-offset: 3px;" class="ques text-decoration-underline primary-text-dark">Question {{index+1}}:</div>
          <div class="btns ">
            <v-btn @click="editQuestion(index)" x-small class="primary-bg-dark">edit</v-btn>
            <v-btn @click="deleteQuestion(index)" x-small class="error">Delete</v-btn>
          </div>
        </div>
        <div class="text-h6 px-3 py-1 pb-3" v-html="question.name"></div>
        <div v-for="(option, index2) in question.options" :key="index2" class="option">
          <v-divider></v-divider>
          <v-list-item v-html="option.name" :class="{'correct':option.correct}"></v-list-item>
        </div>
      </v-list>

    </div>
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
import QuestionPanel from "../../../../../components/QuestionPanel";

export default {
  name: "QuestionBankChapterQuestions",
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
      chapterForm: '',
      isChapterFormValid: true,
      componentKey: 0,
      editableQuestion: null,
      editMode: false,
      snackbar: false,
      snackbarText: null,
    }
  },
  mounted(){
    if(process.browser){
      window.MathJax.Hub.Queue([
        "Typeset",
        window.MathJax.Hub,
        this.$refs.mathJaxEl,
      ]);
    }
  },
  created(){
    // console.log(this.questions);
  },
  async asyncData({ $http, params }){
    let subjects = await $http.$get('/api/subjects');
    let levels = await $http.$get('/api/levels');
    let chapter = await $http.$get(`/api/admin/question-bank/${params.id}/chapters/${params.chapter_id}/get`);
    // let newChapter = await $http.$get(`/api/admin/question-bank/${params.id}/chapters/${params.chapter_id}/get`);
    let questions = await $http.$get(`/api/admin/question-bank/${params.id}/chapters/${params.chapter_id}/questions/get`);
    // let id = params.id;let chapter_id = params.chapter_id;
    let question_bank_id = params.id;
    return { chapter, questions, subjects, levels, question_bank_id };
  },

  methods: {
    async updatechapter(){
      try{
        let res = await this.$axios.post(`/api/admin/question-bank/${this.question_bank_id}/chapters/${this.chapter.id}/edit`, this.chapter);
        this.chapter = res.data;
        this.snackbar = true;
        this.snackbarText = 'Chapter updated successfully';
      }catch(er){
        console.log(er);
      }
    },
    newQuestionCloseWithoutSave(){
      this.newQuestionDialog = false;
    },
    async newQuestionWithSave(question){
      // console.log(question);
      try{
        let res = await this.$axios.post(`/api/admin/question-bank/${this.question_bank_id}/chapters/${this.chapter.id}/questions/add`, question);
        this.$refs.questionPanel.clearQuestions();
        location.reload();
      }catch(er){
        console.log(er);
      }
    },
    async saveEditedQuestion(ques){
      try{
        let res = await this.$axios.post(`/api/admin/question-bank/${this.question_bank_id}/chapters/${this.chapter.id}/questions/${ques.id}/edit`, ques);
        // this.$refs.questionPanel.clearQuestions();
        location.reload();
      }catch(err){
        console.log(err);
      }
    },
    async editQuestion(index){
      this.$refs.questionPanel.setEditMode(this.questions[index]);
      window.scrollTo(0, this.$refs.editor.offsetTop);
    },
    async deleteQuestion(index){
      if(confirm('do you really want to delete this question?')){
        try{
          let ques = this.questions[index];
          let res = await this.$axios.post(`/api/admin/question-bank/${this.question_bank_id}/chapters/${this.chapter.id}/questions/${ques.id}/delete`);
          // this.$router.app.refresh();
          location.reload();
        }catch(err){
          console.log(err);
        }
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
.nuxtbtn{
  text-decoration: none;
  color: white;
}
/*.question-panel{*/
/*  position: absolute;*/
/*  left: 0;*/
/*  top: 0;*/
/*  width: 100vw;*/
/*  height: 100vh;*/
/*}*/
</style>
