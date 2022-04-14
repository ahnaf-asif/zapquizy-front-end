<template>
<v-container class="mt-15">
  <v-btn color="primary" @click="sendMessage()">Send asif a message</v-btn>
</v-container>
</template>

<script>
export default {
  name: "asif",
  data(){
    return{
      apiKey: '0zl+7BwGvHUT9OedsWEFzAHhpDOpKd1eTrY1H41u2CU=',
      clientId: '5fdb90c5-425d-4ecd-b275-c3a67595eccd',
      senderId: '8804445649869',
    }
  },
  methods:{
    async sendMessage(){
      console.log('sending a message...');
      try{
        //const res = await this.$axios.$get(`http://q.biztech.com.bd:6005/api/v2/SendSMS?ApiKey=${this.apiKey}&ClientId=${this.clientId}&SenderId=${this.senderId}&Message=${'this is a normal looking message buddy'}&MobileNumbers=${'01755884404'}&Is_Unicode=${true}&Is_Flash=${true}`);
        const res = await this.$axios.$get(`http://q.biztech.com.bd:6005/api/v2/Balance?ApiKey=${this.apiKey}&ClientId=${this.clientId}`);
        console.log(res);
        console.log('Sent a message');
      }catch(e){
        console.log(e);
      }
    }
  }
}
</script>

<style scoped>

</style>
