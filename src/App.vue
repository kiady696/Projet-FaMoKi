
<template>
  <!-- Header avec bandeau -->
  <header class="header">
    <div class="logo">Famoki</div>
    <div class="menu">
      <button @click="showSection('services')">Nos services</button>
      <button @click="showSection('ia')">IA</button>
      <button @click="showSection('history')">Notre histoire</button>
    </div>
  </header>
  <!-- Affichage conditionnel des sections -->
  <div v-if="currentSection === 'services'">Nos services sont ici...</div>
  <div v-if="currentSection === 'ia'">Des infos sur l'IA...</div>
  <div v-if="currentSection === 'history'">Notre histoire...</div>


    <div class="wrapper">
    </div>

  <main>
    <upload_csv  v-on:data="dataframe=$event" v-on:filename="handleFileUpload" v-on:selectedColumn="handleFileColumns"/>
    
    <ModelSelection v-if="fileUploaded && selectedColumn" 
                    v-bind:y="selectedColumn" 
                    v-bind:filename="filename" 
                    v-on:ResultatAPI="handleFileData" 
                    />
  </main>
</template>



<script>
import Upload_csv from './components/upload_csv.vue';
import upload_csv from './components/upload_csv.vue';
import ModelSelection from './components/ModelSelection.vue';

export default {

  data() {
    return {
      dataframe:null, 
      filename: "",//récupère ce que fares envoie// récupère ce que fares envoie
      APIresponse:"",
      fileUploaded: false,
      //list_target : Array,
      selectedColumn:''
      
      
    }
  },
  methods: {
    handleFileUpload(file) {
      console.log("ceci renvoie le nom du fichier :" ,file)
      this.filename = file; // Met à jour le filename reçu de Upload.vue
      console.log("ceci renvoie le nom du fichier :" ,this.filename)
      this.fileUploaded = true;
    },
    handleFileData(data) {
      console.log("ceci renvoie les données du fichier :" ,data)
      this.APIresponse = data; // Met à jour le filename reçu de Upload.vue
    },
    
    handleFileColumns(column) {
       console.log("ceci renvoie les données du fichier :" ,this.selectedColumn)
       this.selectedColumn = column; // Met à jour le filename reçu de Upload.vue
     }
    
  },
  components : {
          ModelSelection, 
           upload_csv
  }
}
</script>
<style scoped>
/* Header noir */
.header {
  background-color: black;
  color: white;
  padding: 10px 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.logo {
  font-size: 24px;
}

.menu button {
  background-color: transparent;
  color: white;
  border: none;
  margin: 0 10px;
  cursor: pointer;
  font-size: 16px;
}

.menu button:hover {
  text-decoration: underline;
}

main {
  padding: 20px;
}

/* Style conditionnel pour les sections */
div {
  padding: 20px;
  margin-top: 20px;
  border: 1px solid #ccc;
}
</style>