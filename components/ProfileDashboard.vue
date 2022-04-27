<template>
  <v-simple-table>
    <template v-slot:default>
      <tbody>
      <tr v-for="item in items" :key="item.name">
        <td>{{ item.name }}</td>
        <td>{{ item.value }}</td>
      </tr>
      </tbody>
    </template>
  </v-simple-table>
</template>

<script>
export default {
  name: "ProfileDashboard.vue",
  data(){
    return{
      items: [
        {name: 'Joined at', value: '12/12/1220'},
        {name: 'Quizzes Participated', value: 12},
        {name: 'Model Test Packages Purchased', value: 13},
      ],
    }
  }
}
</script>

<style scoped>

</style>
