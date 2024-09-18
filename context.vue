<template>
    <v-container fluid fill-height class="d-flex align-center justify-center pa-4">
      <v-card class="pa-6" max-width="600" elevation="8" rounded="lg">
        <v-card-title class="text-center">
          <h1>Contact Us</h1>
        </v-card-title>
  
        <v-card-text>
          <v-form @submit.prevent="submitForm">
            <v-text-field
              v-model="name"
              label="Full Name"
              required
            ></v-text-field>
            
            <v-text-field
              v-model="email"
              label="Email Address"
              type="email"
              required
            ></v-text-field>
            
            <v-textarea
              v-model="message"
              label="Your Message"
              rows="5"
              required
            ></v-textarea>
  
            <v-btn type="submit" color="primary" class="mt-4">Send</v-btn>
          </v-form>
        </v-card-text>
  
        <v-divider></v-divider>
  
        <v-card-text class="text-center">
          <v-row>
            <v-col cols="12" md="6">
              <v-card>
                <v-card-title>Our Address</v-card-title>
                <v-card-text>
                  <p>123 Main Street</p>
                  <p>City, State, 12345</p>
                  <p>Country</p>
                </v-card-text>
              </v-card>
            </v-col>
            
            <v-col cols="12" md="6">
              <v-card>
                <v-card-title>Contact Details</v-card-title>
                <v-card-text>
                  <p><strong>Phone:</strong> +123 456 7890</p>
                  <p><strong>Email:</strong> contact@example.com</p>
                </v-card-text>
              </v-card>
            </v-col>
          </v-row>
        </v-card-text>
      </v-card>
    </v-container>
  </template>
  
  <script>
  export default {
    data() {
      return {
        name: '',
        email: '',
        message: '',
      };
    },
    methods: {
      submitForm() {
        // Handle form submission logic here
        console.log('Form submitted with:', {
          name: this.name,
          email: this.email,
          message: this.message,
        });
        // Reset form fields
        this.name = '';
        this.email = '';
        this.message = '';
      },
    },
  };
  </script>
  
  <style scoped>
  .v-card {
    background: #ffffff;
  }
  
  .v-card-title h1 {
    margin: 0;
    font-size: 24px;
    color: #333;
  }
  
  .v-text-field,
  .v-textarea {
    margin-bottom: 20px;
  }
  
  .v-btn {
    font-weight: 500;
  }
  
  .v-divider {
    margin: 20px 0;
  }
  </style>
  