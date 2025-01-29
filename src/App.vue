
<template>

  <!-- Pour inclure les logos MaterialUI -->
  <link href="https://cdn.jsdelivr.net/npm/@mdi/font@latest/css/materialdesignicons.min.css" rel="stylesheet">




    <div class="wrapper">
      <!-- Juste pour tester vuetify -->
      <v-file-input label="File input exemple"></v-file-input>
    </div>

  <main>
    <ModelSelection v-bind:y="targetY" v-bind:filename="filename" />
  </main>
  <div>
    <upload_csv v-on:data="dataframe=$event"/>
  </div>
</template>



<script>
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
