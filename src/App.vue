
<template>

  <!-- Pour inclure les logos MaterialUI -->
  <link href="https://cdn.jsdelivr.net/npm/@mdi/font@latest/css/materialdesignicons.min.css" rel="stylesheet">

    <!-- Début conteneur global -->
    <div class="container">

      <!-- Conteneur du composant de login -->
      <div class="login_container" v-if="afficherLogin">
        <!-- Composant de page de login -->
        <Login
          v-on:switch-signup = "switchSignup"


        />

      </div>
      <!-- Fin conteneur composant de login -->

      <!-- Début conteneur composant d'inscription -->
      <div class="signup_container" v-if="afficherInscription">
        <!-- Composant de page d'inscription -->
        <Signup
          v-on:switch-login = "switchLogin"


        />

      </div>
      <!-- Fin conteneur composant d'inscription -->



      <div class="wrapper">
        <!-- Juste pour tester vuetify -->
        <v-file-input label="File input exemple"></v-file-input>
      </div>

    <main>
      <ModelSelection 
        v-bind:y="targetY" 
        v-bind:filename="filename" 
      />
    </main>

    <div>
      <upload_csv 
        v-on:data="dataframe=$event"
      />
    </div>

  </div>
  <!-- Fin conteneur global -->



</template>



<script>
import upload_csv from './components/upload_csv.vue';
import ModelSelection from './components/ModelSelection.vue';
import Login from './components/Login.vue';
import Signup from './components/Signup.vue';

export default {
  data() {
    return {

      // Booleen pour afficher la page de login
      afficherLogin : true, 
      // Booleen pour afficher la page d'inscription
      afficherInscription : false, 


      dataframe:null, 
      "filename": "TEST.csv",//récupère ce que fares envoie
      "targetY": "variety"// récupère ce que fares envoie
    }
  },
  components : {
          ModelSelection, 
          upload_csv, 
          Login, 
          Signup
  }, 
  methods : {

    // Pour switcher depuis page login vers page inscription
    switchSignup(){
      this.afficherLogin = false
      this.afficherInscription = true
    }, 

    // Pour switcher depuis page d'inscription vers page de login
    switchLogin(){
      this.afficherLogin = true
      this.afficherInscription = false
    }


  }
}
</script>


<style>
.container{
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  min-width: 80%;
  justify-content: center;
}

.login_container{
  display: flex;
  flex-direction: column;
  justify-content: center;
}


</style>
