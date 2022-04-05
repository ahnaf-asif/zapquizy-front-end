<template>
    <v-card
    :loading="loading"
    class="mx-auto my-12"
    max-width="374"
    elevation="0"
  >
  
    <v-img
      height="250"
      src="https://i.imgur.com/nAgLoH2.png"
    >
        <!-- <v-badge>
            <v-chip  bottom right color="secondary">
                Computer Science
            </v-chip>
        </v-badge> -->

    </v-img>
    

    <v-card-title style="font-weight: bold;">Introduction to web scraping with Python and Beautiful Soup
        
    </v-card-title> 

    <v-card-text class="v-card-text"> 
        <p>Srijon - 12-11-2021</p>
        পাইথন দিয়ে ওয়েব স্ক্র্যাপইং এর প্রথম পর্ব। এই পর্বে ওয়েব স্ক্র্যাপিং সম্পর্কে একটি ধারনা এবং একটা ওয়েবসাইট থেকে রিকুয়েস্ট মডিউল ব্যাবহার করে সেই ওয়েবসাইটের সোর্স কোড সংগ্রহ করা শিখব।
    </v-card-text>

    <v-card-actions>
      <v-btn
        color="indigo darken-1"
        text
      >
        View Article
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default { 
    name: 'ArticleComponent',
    props: [
        
    ],
    data(){
        return {

        }
    }
}
</script>

<style lang="scss">
.v-card-text{
    font-size: 1em;
}
</style>