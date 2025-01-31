

<template>
  <div>
    <input type="file" @change="handleFileChange" />
    <button @click="uploadFile">Uploader CSV </button>
    <pre>{{ data_df }}</pre>
    



  </div>

  
  <v-select label="Select" v-model="target" 
  :items="list_columns"
></v-select>
 <!-- Afficher l'élément sélectionné -->
 <div v-if="target" >
      <p>Élément sélectionné: {{ target }}</p>
    </div>

 <!-- Button pour envoyer la sélection -->
 <v-btn @click="handleChoiceTarget">Envoyer</v-btn>

  <pre></pre>
</template>



<script>
export default {


  emit:['target'],


  data() {
    return {
      file: null, // Stocke le fichier sélectionné
      data_df: null, // Stocke les données du CSV après traitement
      list_columns:[], //colonnes recupérées via API 
      target:'',
    };
  },
  
  methods: {
    // Méthode pour gérer la sélection du fichier
    handleFileChange(event) {
      this.file = event.target.files[0]; // Récupère le fichier sélectionné
      console.log("affichage du file" , this.file); // Affiche le fichier sélectionné dans la console
    },

    handleChoiceTarget(){
      //Recuperer la target et l'emit
      console.log("afficher la target avant affectation:", this.target)
      this.$emit('target', this.target)


      //$emit('target',this.target)
    },

    // Méthode pour uploader le fichier CSV
    uploadFile() {
      if (!this.file) {
        alert('Aucun fichier sélectionné');
        return;
      }

      //convertir en formData car utile pour l'envoie vers le backend
      const formData = new FormData();
      formData.append('file', this.file);
      console.log("affichage formData", formData)

      

      // Utilisation de `fetch` avec .then() au lieu de await
      fetch('/api/csv', {
        method: 'POST',
        body: formData, 
      })
      .then(response => {
        // Vérifie si la réponse est OK
        if (!response.ok) {
          return Promise.reject('Erreur lors de l\'upload');
        }
        return response.json(); // Récupère la réponse JSON
      })

      .then(data => {

        // Stocke les données dans la variable `data_df`
        this.data_df = data; 
        console.log("affichage data", data);

        // recuperer la liste des colonnes dans le front
        this.list_columns= data.colonnes 

        

      //Vérification de l'importation from backend to frontend
        console.log("affichage colonnes avant recuperation ==>", data.colonnes) 

        //Vérification si la liste des colonnes dans le front est bien importée
        console.log("affichage de la liste des colonnes recupérée dans le front", this.list_columns) 

        //this.$emit('target',this.items)
      })
      .catch(error => {
        console.error(error); // Affiche l'erreur s'il y en a une
        alert('Une erreur s\'est produite');
      });
    },
  },
};


</script>
