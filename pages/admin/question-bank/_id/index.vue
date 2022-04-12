<template>
  <v-container>
    <h1 class="my-5 text-h4 text-center">{{ questionBank.name }}</h1>
    <v-card elevation="1">
      <v-card-text>
        <v-form ref="newQuestionBankForm" v-model="newQuestionBankForm" :valid="isNewQuestionBankFormValid">
          <v-text-field
            v-model="questionBank.name"
            label="Name"
            :rules="[v => !!v || 'Name is required']"
            required></v-text-field>
          <v-select
            v-model="questionBank.level_id"
            :items="levels"
            item-text="name"
            item-value="id"
            :rules="[v => !!v || 'Level is required']"
            label="Level"
            required >
          </v-select>
          <v-select
            v-model="questionBank.subject_id"
            :items="subjects"
            item-text="name"
            item-value="id"
            :rules="[v => !!v || 'Subject is required']"
            label="Subject"
            required >
          </v-select>
          <div class="text-center">
            <v-btn color="primary" large @click="saveQuestionBank()">
              Save Question Bank
            </v-btn>
          </div>
        </v-form>
      </v-card-text>
    </v-card>
    <h1 class="mt-10 mb-5 text-h4 text-center">Add New Chapter</h1>

    <v-card elevation="1">
      <v-card-text>
        <v-form ref="newQuestionBankForm">
          <v-text-field
            v-model="newChapterName"
            label="Chapter Name"
            required></v-text-field>
          <div class="text-center">
            <v-btn @click="addChapter()" color="primary" outlined large>
              Add Chapter
            </v-btn>
          </div>
        </v-form>
      </v-card-text>
    </v-card>

    <h1 class="mt-10 mb-5 text-h4 text-center">Chapters</h1>

    <v-data-table
      :headers="chapterHeaders"
      :items="chapters"
      :items-per-page="10"
      class="elevation-1"
    >
      <template v-slot:item.question_count="{ item }">

        {{item.questions.length}}

      </template>
      <template v-slot:item.enter="{ item }">

        <v-btn small color="success">
          <NuxtLink class="btnlink" :to="`/admin/question-bank/${questionBank.id}/chapters/${item.id}`">Enter</NuxtLink>
        </v-btn>

      </template>
      <template v-slot:item.delete="{ item }">

        <v-btn @click="deleteChapter(item.id)" small color="error">
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
  name: "index",
  auth: true,
  middleware: 'admin',
  layout: 'admin',
  transition: 'fade',
  async asyncData({ $http, params }){
    let subjects = await $http.$get('/api/subjects');
    let levels = await $http.$get('/api/levels');
    let questionBank = await $http.$get(`/api/admin/question-bank/get/${params.id}`);
    let chapters = await $http.$get(`/api/admin/question-bank/${params.id}/chapters/get`);
    return { subjects, levels, questionBank, chapters };
  },
  data(){
    return {
      isNewQuestionBankFormValid: true,
      newQuestionBankForm: '',
      chapterHeaders: [
        { text: 'Chapter Name', value: 'name'},
        { text: 'Questions Added', value: 'question_count'},
        { text: 'Action', value: 'enter'},
        { text: 'Action', value: 'delete'}
      ],
      newChapterName: null,
      snackbar: false,
      snackbarText: null,
    }
  },
  methods: {
    async saveQuestionBank(){
      // console.log(this.questionBank);
      try{
        this.questionBank = await this.$axios.$post(`/api/admin/question-bank/update/${this.questionBank.id}`, this.questionBank);
        // console.log(await this.$axios.$post(`/api/admin/question-bank/update/${this.questionBank.id}`, this.questionBank));
        this.snackbar = true;
        this.snackbarText = 'Question Bank Updated';
      }catch(err){
        console.log(err);
      }
    },
    async addChapter(){
      try{
        if(this.newChapterName){
          this.chapters = await this.$axios.$post(`/api/admin/question-bank/${this.questionBank.id}/chapters/add`, {name: this.newChapterName});
          this.newChapterName = null;
          this.snackbar = true;
          this.snackbarText = 'Successfully added a chapter';
        }
      }catch(err){
        console.log(err);
      }
    },
    async deleteChapter(chapterId){
      if(confirm('Are you sure about deleting this chapter?')){
        try{
          this.chapters = await this.$axios.$post(`/api/admin/question-bank/${this.questionBank.id}/chapters/${chapterId}/delete`);
          this.snackbar = true;
          this.snackbarText = 'Successfully Deleted the chapter';
        }catch(err){
          console.log(err);
        }
      }
    }
  }
}
</script>

<style scoped>
.btnlink{
  text-decoration: none;
  color: white;
}
</style>
