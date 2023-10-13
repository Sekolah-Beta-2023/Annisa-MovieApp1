<template>
    <div class="profile">
      <div class="profile-info" v-if="profile">
        <h1>{{ profile.name }}</h1>
        <p>Email: {{ profile.email }}</p>
        <p>Deskripsi: {{ profile.description }}</p>
      </div>
      <div v-else>
        <p>Loading...</p>
      </div>
    </div>
  </template>
  

<script>
import firebase from "firebase/app";
import "firebase/firestore";

export default {
  async asyncData({ params }) {
    // Inisialisasi Firebase
    if (!firebase.apps.length) {
      firebase.initializeApp({
        apiKey: "AIzaSyA2qfJFOe9aILSwgRyJk15HjV2V_cSI5d0",
  authDomain: "tugas-akhir-4a523.firebaseapp.com",
  databaseURL: "https://tugas-akhir-4a523-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "tugas-akhir-4a523",
  storageBucket: "tugas-akhir-4a523.appspot.com",
  messagingSenderId: "1032284256764",
  appId: "1:1032284256764:web:01a1cf1dd5fe95154f5a79",
  measurementId: "G-YFWDVHPW0Z"
      });
    }

    // Data profil dari Firestore berdasarkan ID
    const db = firebase.firestore();
    try {
      const profileDoc = await db.collection("profiles").doc(params.id).get();
      if (profileDoc.exists) {
        const profile = profileDoc.data();
        return { profile };
      } else {
        // Handle jika dokumen profil tidak ditemukan
        console.error("Dokumen profil tidak ditemukan.");
        return {};
      }
    } catch (error) {
      console.error("Terjadi kesalahan dalam mengambil data profil:", error);
      return {};
    }
  },
};
</script>

<style lang="scss">
.profile {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;

  .profile-info {
    text-align: center;
    background-color: #f0f0f0;
    padding: 20px;
    border-radius: 10px;

    h1 {
      font-size: 24px;
    }

    p {
      margin: 10px 0;
    }
  }
}
</style>
