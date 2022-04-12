<template>
  <v-container>
    <h1 class="my-5 text-h4 text-center">Add New Question Bank</h1>
    <v-card elevation="1">
      <v-card-text>
        <v-form ref="newQuestionBankForm" v-model="newQuestionBankForm" :valid="isNewQuestionBankFormValid">
          <v-text-field
            v-model="newQuestionBankName"
            label="Name"
            :rules="[v => !!v || 'Name is required']"
            required></v-text-field>
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
            <v-btn color="primary" large @click="addQuestionBank">
              Add Question Bank
            </v-btn>
          </div>
        </v-form>
      </v-card-text>
    </v-card>
    <h1 class="mt-10 mb-5 text-h4 text-center">All Question Banks</h1>

    <v-data-table
      :headers="QuestionBankHeaders"
      :items="questionBanks"
      :items-per-page="10"
      class="elevation-1"
    >
      <template v-slot:item.enter="{ item }">

        <v-btn small color="success">
          <NuxtLink class="btnlink" :to="`/admin/question-bank/${item.id}`">Enter</NuxtLink>
        </v-btn>

      </template>
      <template v-slot:item.delete="{ item }">

        <v-btn @click="deleteQuestionBank(item.id)" small color="error">
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
      newQuestionBankName: '',
      newQuestionBankForm: '',
      isNewQuestionBankFormValid: true,
      selectedLevel: null,

      selectedSubject: null,

      QuestionBankHeaders: [
        { text: 'Question Bank Name', value: 'name'},
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
    let questionBanks = await $http.$get('/api/admin/question-bank/get');
    return { subjects, levels, questionBanks };
  },
  methods: {
    async addQuestionBank(){
      if(this.$refs.newQuestionBankForm.validate()){
        let newQuestionBank = {
          name: this.newQuestionBankName,
          level_id: this.selectedLevel,
          subject_id: this.selectedSubject,
        }
        try{
          this.questionBanks = await this.$axios.$post('/api/admin/question-bank/add', newQuestionBank);
          this.snackbar = true;
          this.snackbarText = 'Question Bank Added successfully';
        }catch(err){
          console.log(err);
        }
      }
    },
    async deleteQuestionBank(id){
      if(confirm('Do you really want to delete the Question Bank?')){
        try{
          this.questionBanks = await this.$axios.$post(`/api/admin/question-bank/delete/${id}`);
          this.snackbar = true;
          this.snackbarText = 'Question Bank Deleted successfully';
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
