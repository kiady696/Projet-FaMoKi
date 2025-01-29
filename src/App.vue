
<template>


    <div class="wrapper">
    </div>

  <main>
    <ModelSelection v-bind:y="targetY" v-bind:filename="filename" />
  </main>
  <div>
    <upload_csv v-on:data="dataframe=$event"/>
  </div>
</template>



<script>
import Upload_csv from './components/upload_csv.vue';
import upload_csv from './components/upload_csv.vue';
import ModelSelection from './components/ModelSelection.vue';

export default {
  data() {
    return {
      dataframe:null, 
      "filename": "TEST.csv",//récupère ce que fares envoie
      "targetY": "variety"// récupère ce que fares envoie
    }
  },
  components : {
          ModelSelection, 
           upload_csv
  }
}
</script>
