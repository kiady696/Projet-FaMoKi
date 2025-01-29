<template>


    <div class="wrapper">

      <ModelSelection  />

      <!-- Juste pour tester vuetify -->
      <v-file-input label="File input example"></v-file-input>
     
    </div>


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
      dataframe:null
    }
  },
  components : {
          ModelSelection, 
           upload_csv
  }
}
</script>
