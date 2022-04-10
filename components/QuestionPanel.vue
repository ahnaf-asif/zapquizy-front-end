<template>

  <v-card style="">
    <v-toolbar dark color="primary">
      <v-btn icon dark @click="closeWithoutSave()">
        <v-icon>mdi-close</v-icon>
      </v-btn>
      <v-toolbar-title>Add New Question</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items>
        <v-btn dark text @click="closeWithSave()">
          Save
        </v-btn>
      </v-toolbar-items>
    </v-toolbar>
    <div class="my-5"></div>
    <v-card-text>
      <div class="question-types my-3">
        <v-btn
          color="white--text" x-small
          class="my-1 mx-1"
          :class="{'error':questionType==='mcq', 'teal darken-3':questionType!=='mcq' }"
          @click="changeQuestionType('mcq')"
        >MCQ</v-btn>
        <v-btn
          color="white--text" x-small
          class="my-1 mx-1"
          @click="changeQuestionType('true_false')"
          :class="{'error':questionType==='true_false', 'teal darken-3':questionType!=='true_false' }"
        >True/False</v-btn>
        <v-btn
          color="white--text" x-small
          class="my-1 mx-1"
          @click="changeQuestionType('short_ans')"
          :class="{'error':questionType==='short_ans', 'teal darken-3':questionType!=='short_ans' }"
        >Short Answer</v-btn>
      </div>
      <div class="mcq" v-if="questionType === 'mcq'">
        <client-only placeholder="loading component...">
        <h1 class="text-h6 my-1 ">Question: </h1>
        <ckeditor-nuxt
          v-model="mcqQuestion.name"
          :config="editorConfig"
        />
        <div class="spacer mt-5"></div>
        <div class="testing" v-html="mcqQuestion.name"></div>
        <v-alert v-if="err" dense type="error"
        >{{ errMsg }}</v-alert>
        <div v-for="(option, index) in mcqQuestion.options" :key="index" class="option">
          <h1 class="text-h6 my-1">option {{index+1}}: </h1>
          <v-row>
            <v-col cols="12" md="10">
              <ckeditor-nuxt
                v-model="mcqQuestion.options[index].name"
                :config="editorConfig"
              />
            </v-col>
            <v-col class="mcq-option-btns" cols="12" md="2">
              <v-btn class="mcq-option-btn" small
                :class="{
                  'primary':!mcqQuestion.options[index].correct,
                  'success':mcqQuestion.options[index].correct
                }"
                @click="markMcqCorrect(index)"
              >
                  <span v-if="!mcqQuestion.options[index].correct">Mark as Correct</span>
                  <span v-else>Correct</span>
              </v-btn>
              <v-btn class="mcq-option-btn" small color="error">Remove Option</v-btn>
            </v-col>
          </v-row>
        </div>
        </client-only>
      </div>
    </v-card-text>
    <client-only placeholder="loading component...">
      <v-card-actions class="flex flex-row justify-center">
        <p v-if="questionType==='mcq'" class="mcq-actions">
          <v-btn outlined color="black text--white" @click="addMcqOption()">Add new Option</v-btn>
        </p>
      </v-card-actions>
    </client-only>
  </v-card>


</template>

<script>
export default {
  name: "QuestionPanel.vue",
  components: {
    'ckeditor-nuxt': () => { if (process.client) { return import('@blowstack/ckeditor-nuxt') } },
  },
  data(){
    return {
      contentHolder: '',
      editorConfig: {
        removePlugins: ['Title'],
      },
      questionType: 'mcq',

      mcqQuestion: {
        type: 'mcq',
        name: '',
        options: [],
      },
      trueFalseQuestion: {},
      shortAnsQuestion: {},
      err: false,
      errMsg: true,
    }
  },
  methods: {
    mcqQuestionValid(){
      if(this.mcqQuestion.name == null || this.mcqQuestion.name === ""){
        this.err = true;
        this.errMsg = 'Question can not be empty';
        return false;
      }
      let ase = false,cnt=false;
      this.mcqQuestion.options.forEach(option => {
        if(option.name != null || option.name !== "")ase=true;
        if(option.correct === true)cnt=true;
      });
      if(!ase){
        this.err = true;
        this.errMsg = 'Option can not be empty';
      }
      if(!cnt){
        this.err = true;
        this.errMsg = 'At least one option must be correct';
      }
      return ase && cnt;
    },
    closeWithSave(){
      this.err = false;
      if(this.mcqQuestionValid()){
        this.$emit('close-with-save', this.mcqQuestion);
      }
    },
    closeWithoutSave(){
      this.$emit('close-without-save');
    },
    changeQuestionType(str){
      this.questionType = str;
    },
    addMcqOption(){
      this.mcqQuestion.options.push({
        name: null,
        correct: false,
      });
    },
    markMcqCorrect(index){
      this.mcqQuestion.options[index].correct = !this.mcqQuestion.options[index].correct;
    },
    clearQuestions(){
      this.mcqQuestion = {
        type: 'mcq',
        name: '',
        options: [],
      };
    }
  },
}
</script>

<style scoped>
@media(max-width: 760px){
  .mcq-option-btns{
    padding-top: 0;
    display: flex;
    justify-content: space-between;
  }
  .mcq-option-btn{
    /*width: 100%;*/
  }
}
@media(min-width: 760px){
  .mcq-option-btn{
    height: 40px !important;
    width: 180px;
  }
  .mcq-option-btns{
    display: flex;
    flex-direction: column;
    justify-content: center;
    gap: 5px;
  }
}
</style>
