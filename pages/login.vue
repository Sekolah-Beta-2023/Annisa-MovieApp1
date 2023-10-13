<template>
    <div class="login-container">
      <form @submit.prevent="login" class="login-form">
        <div class="form-group">
          <label for="email">Email:</label>
          <input v-model="email" type="email" id="email" required />
        </div>
        <div class="form-group">
          <label for="password">Password:</label>
          <input v-model="password" type="password" id="password" required />
        </div>
        <div class="form-group">
          <button type="submit" class="custom-button">Login</button>
        </div>
      </form>
    </div>
  </template>
  
  <script>
  import firebase from 'firebase/app';
import 'firebase/auth';

export default {
  data() {
    return {
      email: '',
      password: '',
      user: null,
    };
  },
  methods: {
    async login() {
      try {
        const { user } = await firebase.auth().signInWithEmailAndPassword(
          this.email,
          this.password
        );

        console.log('Logged in:', user);
        this.user = user;
        this.$router.push('/');
      } catch (error) {
        console.error('Login failed:', error.message);
      }
    },
  },
    created() {
      const config = {
        apiKey: "AIzaSyA2qfJFOe9aILSwgRyJk15HjV2V_cSI5d0",
  authDomain: "tugas-akhir-4a523.firebaseapp.com",
  databaseURL: "https://tugas-akhir-4a523-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "tugas-akhir-4a523",
  storageBucket: "tugas-akhir-4a523.appspot.com",
  messagingSenderId: "1032284256764",
  appId: "1:1032284256764:web:01a1cf1dd5fe95154f5a79",
      };
  
      if (!firebase.apps.length) {
        firebase.initializeApp(config);
      }
  
      // Periksa jika pengguna sudah masuk
      firebase.auth().onAuthStateChanged((user) => {
        if (user) {
          // Pengguna sudah masuk, izinkan akses ke halaman utama
          this.user = user;
          this.$router.push('/index');
        } else {
          // Pengguna belum masuk, biarkan mereka di halaman login
          this.$router.push('/login');
        }
      });
    },
  };
  </script>
  <style lang="scss" scoped>
  .login-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
  }
  
  .login-form {
    width: 300px;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
    background-color: #f5f5f5;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  }
  
  .form-group {
    margin-bottom: 10px;
  }
  
  label {
    font-weight: bold;
  }
  
  input[type="email"],
  input[type="password"] {
    width: 100%;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 3px;
  }
  
  .custom-button {
    width: 100%;
    padding: 10px;
    background-color: #007bff;
    color: #fff;
    border: none;
    border-radius: 3px;
    cursor: pointer;
  
    &:hover {
      background-color: #0056b3;
    }
  }
  </style>