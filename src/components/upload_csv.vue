

<template>
    <div>
      <input type="file" @change="handleFileChange" />
      <button @click="uploadFile">Uploader CSV </button>
      <pre>{{ data_df }}</pre>
    </div>
    <v-select
      v-model="selectedColumn"
      :items="columnHeaders"   
      label="Sélectionner la colonne cible">
      </v-select>
      <p> element selectionné : {{ selectedColumn }}</p>
      <!--Bouton pour soumettre la requête-->
      <v-btn @click="uploadTarget">Valider la target</v-btn>
      

    <pre></pre>
  </template>
  
  
  
  <script>
  export default {


    emits:['data','filename','columnHeaders','selectedColumn'],


    data() {
      return {
        file: null, // Stocke le fichier sélectionné
        data_df: null, // Stocke les données du CSV après traitement
        columnHeaders:[],
        selectedColumn : ''
      };
    },
    methods: {
      // Méthode pour gérer la sélection du fichier
      handleFileChange(event) {
        this.file = event.target.files[0]; // Récupère le fichier sélectionné
        console.log("affichage du file" , this.file); // Affiche le fichier sélectionné dans la console
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
          this.data_df = data; // Stocke les données dans la variable `data`
          console.log(data); // Affiche les données dans la console
          // Convertir la chaîne JSON `data_preview` en objet
          const parsedData = JSON.parse(data.data_preview);
          console.log(parsedData);
          // Extraire les clés des colonnes (en-têtes)
          this.columnHeaders = data.colonnes;
          console.log("Colonnes extraites :", this.columnHeaders);
          this.$emit('data',data)
          this.$emit('filename',data.filename)
          this.$emit('columnHeaders', this.columnHeaders)
          this.$emit('selectedColumn',this.selectedColumn)
        })
        .catch(error => {
          console.error(error); // Affiche l'erreur s'il y en a une
          alert('Une erreur s\'est produite');
        });
      },

      uploadTarget() {
        this.$emit('selectedColumn',this.selectedColumn)
      }
    },
  };
  </script>
  <style scoped>
  .upload-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 20px;
  }
  
  button {
    margin-top: 10px;
    padding: 10px 20px;
    background-color: black;
    color: white;
    border: none;
    cursor: pointer;
  }
  
  button:hover {
    background-color: #333;
  }
  </style>
  