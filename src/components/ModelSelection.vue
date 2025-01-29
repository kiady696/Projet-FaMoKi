<!--Partie HTML*/-->  
<template>
<!--[Selection du Model]*/ -->
    <v-container> <!-- création de a liste déroulante affichant les modèles-->
      <v-select  
        v-model="model"
        :items="models" 
        label="Sélectionner un modèle"
      ></v-select>
      <!--Bouton pour soumettre la requête-->
      <v-btn @click="selectModel">Entraîner le modèle</v-btn>
    </v-container>
  </template>
  <!-- Partie JS-->
  <script>
  /*export default {
    //Dans data je mets une liste (models) qui permet de récupérer l'ensemble des models qui existent sur python
    //J'ajoute le model qui sera sélectionné par l'utilisateur (model)
    data() {
      return {
        models: ["random_forest", "xgboost"],
        model: "xgboost",
      };
    },

    methods: {
      async selectModel() {
        try {
          const response = await fetch("/api/train_model", {
            method: "POST",
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify({
              model: this.model,
              idFile: 'TEST',
              yColumn: 'variety',
            }),
          });
  
          const data = await response.json();
          if (data.success) {
            this.$router.push({ name: "results" }); // Redirige vers les résultats
          } else {
            alert("Erreur.");
          }
        } catch (error) {
          console.error(error);
          alert("Erreur lors de la sélection du modèle.");
        }
      },
    },*/
    export default {
      props: {
        filename: "", //récupère la réponse de fares reçu dans l'app.vue
        y: "",//récupère la réponse de fares reçu dans l'app.vue
      },
  data() {
    return {
      models: ["random_forest", "xgboost"], // Liste des modèles disponibles
      model: "xgboost", // Modèle sélectionné par défaut
    };
  },

  methods: {
    selectModel() {
      // Préparer les données à envoyer à l'API
      const payload = {
        model: this.model,
        idFile: "TEST",
        yColumn: "variety",
      };
/*
      // Effectuer une requête POST simple
      const xhr = new XMLHttpRequest();
      xhr.open("POST", "/api/train_model", true);
      xhr.setRequestHeader("Content-Type", "application/json");

      // Définir la gestion de la réponse
      xhr.onload = function () {
        if (xhr.status === 500) {

          const response = JSON.parse(xhr.responseText);
          if (response.success) {
            alert("Modèle entraîné avec succès !");
          } else {
            alert("Erreur lors de l'entraînement du modèle.");
          }
        } else {
          alert("Erreur : Impossible de se connecter à l'API.");
        }
      };

      // Gérer les erreurs réseau
      xhr.onerror = function () {
        alert("Erreur réseau. Vérifiez votre connexion.");
      };

      // Envoyer la requête
      xhr.send(JSON.stringify(payload));
    },*/
    // Appel à mon API
      fetch(`/api/train_model`, {method : "POST", 
      body : {
        TRUCBidule:this.filename, //Donne la réponse JSON reçu dans app.vue à python
        target:this.y,//Donne la réponse JSON reçu dans app.vue à python
      model: this.model}
    } )
      .then(response => response.json())
      .then(data => {
          console.log(data);
          if (data.length === 0) {
              response.innerHTML = `<p>Aucune donné pour mon api".</p>`;
              return data;
          }
          
      })
      .catch(error => {
          console.error("Erreur lors de la récupération de l'api :", error);
          // albumsContainer.innerHTML = `<p>Une erreur est survenue.</p>`;/* */
      });
    },
  } 
};

  </script>
  