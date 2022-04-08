<template>
  <v-container>
    <AdminDashboard></AdminDashboard>
  </v-container>
</template>

<script>
import AdminDashboard from "../../components/AdminDashboard";
export default {
  name: "index",
  auth: true,
  middleware: 'admin',
  layout: 'admin',
  transition: 'fade',
  components: {
    AdminDashboard
  },
  data(){
    return {

    }
  }
}
</script>

<style scoped>

</style>
