<template>
  <v-container class="d-flex align-center justify-center" style="height: 100vh; background-color: #e0e7ec;">
    <v-card
      class="pa-10 pb-10"
      elevation="12"
      max-width="600"
      rounded="xl"
      style="background: #ffffff; border-radius: 20px;"
    >
      <div class="text-center mb-10">
        <h1 class="font-weight-bold mb-6" style="color: #2c3e50;">Sign In</h1>
      </div>

      <v-form>
        <v-text-field
          v-model="email"
          density="comfortable"
          placeholder="Email address"
          prepend-inner-icon="mdi-email-outline"
          variant="outlined"
          color="primary"
          class="mb-8"
          outlined
          label="Email"
          hide-details
        ></v-text-field>

        <v-text-field
          v-model="password"
          :append-inner-icon="visible ? 'mdi-eye-off' : 'mdi-eye'"
          :type="visible ? 'text' : 'password'"
          density="comfortable"
          placeholder="Enter your password"
          prepend-inner-icon="mdi-lock-outline"
          variant="outlined"
          color="primary"
          outlined
          label="Password"
          hide-details
          class="mb-6"
          @click:append-inner="visible = !visible"
        ></v-text-field>

        <div v-if="errorMessage" class="mb-6">
          <v-alert type="error" dismissible>{{ errorMessage }}</v-alert>
        </div>

        <div class="text-right mb-10">
          <a
            class="text-caption text-decoration-none"
            style="color: #3498db; font-size: 1.1rem;"
            href="#"
            rel="noopener noreferrer"
            target="_blank"
          >
            Forgot password?
          </a>
        </div>

        <v-btn
          color="primary"
          class="mb-6"
          size="x-large"
          variant="elevated"
          block
          style="border-radius: 16px; text-transform: none; font-weight: bold; font-size: 1.2rem;"
          @click="doLogin"
        >
          Log In
        </v-btn>

        <v-btn
          color="secondary"
          size="x-large"
          variant="outlined"
          block
          style="border-radius: 16px; text-transform: none; font-weight: bold; font-size: 1.2rem;"
          @click="goToRegister"
        >
          Sign Up
        </v-btn>
      </v-form>
    </v-card>
  </v-container>
</template>

<script>
import axios from 'axios';

definePageMeta({
  layout: "#",
});

export default {
  data() {
    return {
      visible: false,
      email: '',
      password: '',
      errorMessage: '' // เพิ่มตัวแปรสำหรับข้อความข้อผิดพลาด
    };
  },
  methods: {
    async doLogin() {
      this.errorMessage = ''; // รีเซ็ตข้อความข้อผิดพลาด
      let forms = {
        email: this.email,
        password: this.password
      };
      try {
        const response = await axios.post('http://localhost:7000/login', forms);
        const data = response.data;
        if (data.status === 'success') {
          window.sessionStorage.setItem('token', JSON.stringify(data.token));
          this.$router.replace('/home');
        } else {
          this.errorMessage = 'กรุณากรอกอีเมล & รหัสผ่านให้ถูกต้อง'; // แสดงข้อความข้อผิดพลาด
        }
      } catch (error) {
        this.errorMessage = 'กรุณากรอกอีเมล และ รหัสผ่านให้ถูกต้อง'; // แสดงข้อความข้อผิดพลาด
        console.error('Login error:', error);
      }
    },
    goToRegister() {
      this.$router.push('/signup');
    }
  }
};
</script>

<style scoped>
.text-center {
  text-align: center;
  padding-left: 200px;
  padding-right: 200px;
}

.mb-6 {
  margin-bottom: 1.5rem;
}

.mb-8 {
  margin-bottom: 2rem;
}

.mb-10 {
  margin-bottom: 2.5rem;
}

.text-caption {
  font-size: 1.1rem;
}

.text-decoration-none {
  text-decoration: none;
}

.font-weight-bold {
  font-weight: bold;
}
</style>
