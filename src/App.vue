
<template>


    <div class="wrapper">
    </div>

  <main>
    <ModelSelection v-bind:y="targetY" v-bind:filename="filename" />
  </main>
  <div>
    <upload_csv v-on:data="dataframe=$event" v-on:target="handleNewTarget"/>
    
  </div>

</template>



<script>
import ModelSelection from './components/ModelSelection.vue';
import Upload_csv from './components/upload_csv.vue';

export default {

  data() {
    return {
      dataframe:null, 
      "filename": "TEST.csv",//récupère ce que fares envoie
      "targetY": "variety",// récupère ce que fares envoie
      target:''
    }
  },
  components : {
          ModelSelection, 
          Upload_csv
  },
  methods:{
    handleNewTarget(){
      this.target=target
    }
  }
}

</script>
