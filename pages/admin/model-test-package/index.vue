<template>
  <v-container>
    <h1 class="my-5 text-h4 text-center">Add New Model Test Package</h1>
    <v-card elevation="1">
      <v-card-text>
        <v-alert v-if="submitError" type="error">{{submitErrorText}}</v-alert>
        <v-form ref="newModelTestForm">
          <v-text-field
            v-model="newName"
            label="Name"
            :rules="[v => !!v || 'Name is required']"
            hint="Try to set an unique name (not necessary, but recommended)"
            required></v-text-field>
          <v-text-field
            v-model="price"
            label="Package Price (in tk)"
            :rules="[v => !!v || 'Price is required']"
            type="number"
            required></v-text-field>
          <v-file-input
            v-model="coverImage"
            label="Model Test Package Cover Image"
            show-size
            accept="image/*"
            append-icon="mdi-camera"
            prepend-icon=""
            required></v-file-input>
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
          <v-text-field
            v-model="authorName"
            label="Author Name"
            :rules="[v => !!v || 'Author name is required']"
            type="text"
            required></v-text-field>
          <v-text-field
            v-model="authorDesignation"
            label="Author Designation"
            :rules="[v => !!v || 'Author designation is required']"
            type="text"
            required></v-text-field>
          <v-file-input
            v-model="authorPicture"
            label="Author Picture"
            show-size
            accept="image/*"
            append-icon="mdi-camera"
            prepend-icon=""
            required></v-file-input>
          <div class="text-center">
            <v-btn color="primary" large @click="addModelTestPackage()">
              Add Model Test Package
            </v-btn>
          </div>
        </v-form>
      </v-card-text>
    </v-card>
    <h1 class="mt-10 mb-5 text-h4 text-center">All Model Test Packages</h1>
    <v-data-table
      :headers="packageHeaders"
      :items="modelTestPackages"
      :items-per-page="10"
      class="elevation-1"
    >
      <template v-slot:item.enter="{ item }">

        <v-btn small :to="`/admin/model-test-package/${item.id}`" color="success">
          Enter
        </v-btn>

      </template>
      <template v-slot:item.delete="{ item }">

        <v-btn @click="deleteModelTestPackage(item.id)" small color="error">
          Delete
        </v-btn>

      </template>
      <template v-slot:item.status="{ item }">

        <span v-if="item.status" class="green--text">Active</span>
        <span v-else class="red--text">Inactive</span>

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
  async asyncData({ $http }){
    let subjects = await $http.$get('/api/subjects');
    let levels = await $http.$get('/api/levels');
    let modelTestPackages = await $http.$get('/api/admin/model-test-package/get');
    return { subjects, levels, modelTestPackages };
  },
  data(){
    return {
      newName: null,
      selectedSubject: null,
      selectedLevel: null,
      coverImage: null,
      price: null,
      authorName: null,
      authorDesignation: null,
      authorPicture: null,

      submitError: false,
      submitErrorText: '',
      packageHeaders: [
        { text: 'Package Name', value: 'name'},
        { text: 'Price', value: 'price'},
        { text: 'Subject', value: 'subject.name'},
        { text: 'Level', value: 'level.name'},
        { text: 'status', value: 'status'},
        { text: 'Action', value: 'enter'},
        { text: 'Action', value: 'delete'},
      ],
      snackbar: false,
      snackbarText: null,
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
        const coverImageUploaded = await this.$axios.post('/api/image/upload', data, config);
        // console.log(res);

        const data2 = new FormData();

        data2.append('file', this.authorPicture);
        const authorPictureUploaded = await this.$axios.post('/api/image/upload', data2, config);
        // console.log(res2);
        // return;

        if(res.status === 200 && res2.status === 200){
          let newModelTestPackage = {
            name: this.newName,
            price: this.price,
            subject_id: this.selectedSubject,
            level_id: this.selectedLevel,
            cover_image: coverImageUploaded.data.file_path,
            author_name: this.authorName,
            author_designation: this.authorDesignation,
            author_picture: this.authorPictureUploaded.data.file_path
          };
          try{
            // console.log(newModelTestPackage);
            const res = await this.$axios.post('/api/admin/model-test-package/add', newModelTestPackage);
            // console.log(res.data);
            this.modelTestPackages = res.data;
            this.snackbar = true;
            this.snackbarText = 'Model Test Package added';
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

    }
  }
}
</script>

<style scoped>

</style>
