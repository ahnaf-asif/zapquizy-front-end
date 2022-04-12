<template>
  <v-container>
    <br><br><br>
    <client-only>
      <ckeditor-nuxt
        v-model="contentHolder"
        :config="editorConfig"
      />
    </client-only>
  </v-container>
</template>

<script>

export default {
  name: "demo.vue",
  layout: 'Demo',
  components: {
    'ckeditor-nuxt': () => { if (process.client) { return import('@blowstack/ckeditor-nuxt') } },
  },
  data(){
    return {
      contentHolder: '',
      editorConfig: {
        removePlugins: ['Title','MediaEmbedToolbar'],
        toolbar_Custom: [],
      },
    }
  }
}
</script>

<style scoped>

</style>
