<!--Partie HTML*/-->  
<template>
<!--[Selection du Model]*/ -->
    <v-container> <!-- création de a liste déroulante affichant les modèles-->
      <v-select  
        v-model="model"
        :items="models" 
        label="Sélectionner un modèle"
      ></v-select>
      <v-btn @click="selectModel">Entraîner le modèle</v-btn>
      
    </v-container>
    <!-- Animation de chargement lorsque isLoading est vrai -->
     <div v-if="isLoading" class="loading-container">
    <v-progress-circular
      indeterminate
      color="primary"
      size="64"
      class="ma-5"
    ></v-progress-circular>
    <v-alert type="info" class="ma-5" elevation="2">
        Patientez un tout petit instant...
      </v-alert>
    </div>
    <!-- Affichage du graphique -->
    <div v-if="graphique">
      <h2>Graphique des accuracies</h2>
      <img :src="'data:image/png;base64,' + graphique" alt="Accuracy Plot" />
    </div>

    <!-- Affichage des résultats de performance -->
    <div v-if="results">
      <h2>Résultats du modèle</h2>
      <p>Meilleure précision : {{ results.max_accuracy }}</p>
      <p>Nombre optimal d'estimateurs : {{ results.best_n_estimators }}</p>
    </div>
  </template>
  <!-- Partie JS-->
  <script>
    export default {

      emits:['ResultatAPI'],

      props: {
        filename: "", //récupère la réponse de fares reçu dans l'app.vue
        y: "",//récupère la réponse de fares reçu dans l'app.vue
      },
  data() {
    return {
      models: ["random_forest", "xgboost"], // Liste des modèles disponibles
      model: "xgboost", // Modèle sélectionné par défaut
      results :"",
      graphique : "",
      isLoading: false,
    };
  },
  

  methods: {
    selectModel() {
      // Préparer les données à envoyer à l'API
      this.isLoading = true; // Démarre l'animation de chargement
      const payload = {
        model: this.model,
        idFile: this.filename,
        yColumn: this.y, //"variety",
      };
      console.log("mon payload", payload)
     
    // Appel à mon API
    console.log(this.filename)  
    fetch(`/api/train_model`, {method : "POST", 
      body : JSON.stringify(payload),
      headers : {
      'Content-Type': 'application/json',
    }
    } )
    
      .then(response => response.json())
      .then(data => {
          console.log(data);
          if (data.length === 0) {
              response.innerHTML = `<p>Aucune donné pour mon api".</p>`;
              return data;
          }
      this.$emit('ResultatAPI',data)
      this.results = {
        max_accuracy: data.max_accuracy,
        best_n_estimators: data.best_n_estimators,
      };

      // Stocke l'image du graphique en base64
      this.graphique = data.accuracy_plot;
      this.isLoading = false; // Démarre l'animation de chargement
          
      })
      .catch(error => {
          console.error("Erreur lors de la récupération de l'api :", error);
          this.isLoading= false;
          // albumsContainer.innerHTML = `<p>Une erreur est survenue.</p>`;/* */
      });
    },
  } 
};

  </script>

<style scoped>
.v-container {
  margin-top: 20px;
}

v-btn {
  margin-top: 20px;
}

h2 {
  margin-top: 20px;
}
.loading-container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
</style>
  