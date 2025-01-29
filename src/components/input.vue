

<template>
    <div>
      <input type="file" @change="handleFileChange" />
      <button @click="uploadFile">Uploader CSV</button>
      <pre>{{ data }}</pre>
    </div>
  </template>
  
  
  
  <script>
  export default {
    data() {
      return {
        file: null, // Stocke le fichier sélectionné
        data: null, // Stocke les données du CSV après traitement
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
          this.data = data; // Stocke les données dans la variable `data`
          console.log(data); // Affiche les données dans la console
        })
        .catch(error => {
          console.error(error); // Affiche l'erreur s'il y en a une
          alert('Une erreur s\'est produite');
        });
      },
    },
  };
  </script>
  