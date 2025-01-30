<!-- frontend/src/components/Signup.vue -->
<template>
    <v-container fluid fill-height>
      <v-row align="center" justify="center">
        <v-col cols="12" sm="8" md="4">
          <v-card class="elevation-12">
            <v-card-title>Inscription</v-card-title>
            <v-card-text>
              <v-form ref="form" v-model="isFormValid" @submit.prevent="handleSignup">
                <!-- Username field -->
                <v-text-field
                  v-model="username"
                  :rules="usernameRules"
                  label="Username"
                  name="username"
                  prepend-icon="mdi-account"
                  required
                ></v-text-field>
  
                <!-- Password field -->
                <v-text-field
                  v-model="password"
                  :rules="passwordRules"
                  label="Password"
                  name="password"
                  prepend-icon="mdi-lock"
                  :type="showPassword ? 'text' : 'password'"
                  :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
                  @click:append="showPassword = !showPassword"
                  required
                ></v-text-field>
  
                <!-- Confirm Password field -->
                <v-text-field
                  v-model="confirmPassword"
                  :rules="confirmPasswordRules"
                  label="Confirm Password"
                  name="confirmPassword"
                  prepend-icon="mdi-lock-check"
                  :type="showPassword ? 'text' : 'password'"
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
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="primary"
                :disabled="!isFormValid"
                :loading="loading"
                @click="handleSignup"
              >
                Sign Up
              </v-btn>
            </v-card-actions>
            <v-card-text class="pa-0">
            
            <v-btn
              prepend-icon="mdi-account-circle"
              variant="text"
              class="blue w-100 text-button"
              height="48"
              @click="$emit('switchLogin')"
            >
              Se Connecter >
            </v-btn>

        </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script>
  export default {
    name: 'Signup',
    emits: ['switchLogin'], 
    data() {
      return {
        isFormValid: false,
        loading: false,
        username: '',
        password: '',
        confirmPassword: '',
        showPassword: false,
        error: null,
        usernameRules: [
          v => !!v || 'Username is required',
          v => v.length >= 3 || 'Username must be at least 3 characters'
        ],
        passwordRules: [
          v => !!v || 'Password is required',
          v => v.length >= 6 || 'Password must be at least 6 characters'
        ]
      }
    },
    computed: {
      confirmPasswordRules() {
        return [
          v => !!v || 'Please confirm your password',
          v => v === this.password || 'Passwords do not match'
        ]
      }
    },
    methods: {
      handleSignup() {
        if (!this.$refs.form.validate()) {
          return;
        }
        
        this.loading = true;
        this.error = null;
  
        fetch('http://localhost:5000/api/signup', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            username: this.username,
            password: this.password
          })
        })
        .then(response => {
          if (!response.ok) {
            throw new Error('Signup failed');
          }
          return response.json();
        })
        .then(data => {
          // Optionally auto-login after signup
          localStorage.setItem('token', data.token);
          this.$router.push('/dashboard');
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