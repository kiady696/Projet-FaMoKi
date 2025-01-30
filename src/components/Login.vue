<!-- frontend/src/components/Login.vue -->
<template>
    <v-container fluid fill-height>
      <v-row align="center" justify="center">
        <v-col cols="12" sm="8" md="4">
          <v-card class="elevation-12">
            <v-card-title>Connexion</v-card-title>
            <v-card-text>
              <v-form ref="form" v-model="isFormValid" @submit.prevent="handleLogin">
                <v-text-field
                  v-model="email"
                  :rules="emailRules"
                  label="Email"
                  name="email"
                  prepend-icon="mdi-account-circle"
                  type="email"
                  required
                ></v-text-field>
  
                <v-text-field
                  v-model="password"
                  :rules="passwordRules"
                  label="Password"
                  name="password"
                  prepend-icon="mdi-lock"
                  type="password"
                  required
                ></v-text-field>
  
                <v-alert
                  v-if="error"
                  type="error"
                  dense
                  text
                  class="mb-3"
                >
                  {{ error }}
                </v-alert>
              </v-form>
            </v-card-text>
            <div class="d-flex flex-column align-center justify-center">
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  color="primary"
                  variant="elevated"
                  class="pl-pr-4"
                  :disabled="!isFormValid"
                  :loading="loading"
                >
                  Se connecter
                </v-btn>
              </v-card-actions>
            </div>
            <v-card-text class="pa-0">
            
              <v-btn
                prepend-icon="mdi-account-circle"
                variant="text"
                class="blue w-100 text-button"
                height="48"
                @click="$emit('switchSignup')"
              >
                S'inscrire >
              </v-btn>

          </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script>
  export default {
    name: 'Login',
    emits: ['switchSignup'], 
    data() {
      return {
        isFormValid: false,
        loading: false,
        email: '',
        password: '',
        error: null,
        emailRules: [
          v => !!v || 'Email is required',
          v => /.+@.+\..+/.test(v) || 'Email must be valid'
        ],
        passwordRules: [
          v => !!v || 'Password is required',
          v => v.length >= 6 || 'Password must be at least 6 characters'
        ]
      }
    },
    methods: {

      handleLogin() {

        // First validate the form
        if (!this.$refs.form.validate()) {
          return;
        }
        
        this.loading = true;
        this.error = null;
  
        fetch('/api/login', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            email: this.email,
            password: this.password
          })
        })
        .then(response => {
          if (!response.ok) { // Si il ya des erreurs
            throw new Error('Invalid credentials');
          }
          return response.json();
        })
        .then(data => { // Si c'est ok, on utilise le router de vue pour aller vers le dashboard
          localStorage.setItem('token', data.token);
          //this.$router.push('/dashboard'); 

          // On emit un booleen pour dire de masquer le div de login et d'afficher tout le reste

        })
        .catch(err => {
          this.error = err.message;
        })
        .finally(() => {
          this.loading = false;
        });
      },
      
      resetForm() {
        this.$refs.form.reset();
        this.error = null;
      }
    }
  }
  </script>