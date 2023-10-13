<template>
    <div>
      <NavbarWeb />
      <h1>Community Chat</h1>
      <div class="chat-box">
        <div v-for="message in chatMessages" :key="message.id">
          <p :class="{'my-message': message.sender === username}">
            <strong>{{ message.sender }}</strong>: {{ message.message }}
            <span @click="deleteMessage(message.id)" class="delete-button">x</span>
          </p>
        </div>
      </div>
      <div>
        <input v-model="newMessage" @keyup.enter="sendMessage" placeholder="Type your message" />
        <button @click="sendMessage" class="send-button">✈️</button>
      </div>
    </div>
  </template>
  
  <script>
import firebase from 'firebase/app';
import 'firebase/firestore';

export default {
  data() {
    return {
      chatMessages: [],
      newMessage: '',
      username: '',
    };
  },
  methods: {
    async sendMessage() {
      if (this.newMessage.trim() === '') return;

      const db = firebase.firestore();
      const timestamp = firebase.firestore.FieldValue.serverTimestamp();

      try {
        await db.collection('chat').add({
          sender: this.username,
          message: this.newMessage,
          timestamp,
          showDelete: false,
        });
        this.newMessage = '';
      } catch (error) {
        console.error('Error sending message:', error);
      }
    },
    async deleteMessage(messageId) {
      const db = firebase.firestore();

      try {
        await db.collection('chat').doc(messageId).delete();
      } catch (error) {
        console.error('Error deleting message:', error);
      }
    },
    toggleDeleteButton(messageId) {

      const message = this.chatMessages.find(msg => msg.id === messageId);
      if (message) {
        message.showDelete = !message.showDelete;
      }
    },
  },
    async mounted() {

      // Konfigurasi Firebase
      const firebaseConfig = {
        apiKey: "AIzaSyA2qfJFOe9aILSwgRyJk15HjV2V_cSI5d0",
        authDomain: "tugas-akhir-4a523.firebaseapp.com",
        databaseURL: "https://tugas-akhir-4a523-default-rtdb.asia-southeast1.firebasedatabase.app",
        projectId: "tugas-akhir-4a523",
        storageBucket: "tugas-akhir-4a523.appspot.com",
        messagingSenderId: "1032284256764",
        appId: "1:1032284256764:web:01a1cf1dd5fe95154f5a79",
      };
  
      firebase.initializeApp(firebaseConfig);
  
      const db = firebase.firestore();
  
      try {
        const querySnapshot = await db.collection('chat').orderBy('timestamp').get();
        this.chatMessages = querySnapshot.docs.map(doc => doc.data());
      } catch (error) {
        console.error('Error fetching chat messages:', error);
      }
  
      // Langganan perubahan
    db.collection('chat').orderBy('timestamp').onSnapshot(querySnapshot => {
      querySnapshot.docChanges().forEach(change => {
        if (change.type === 'added') {
          const data = { id: change.doc.id, ...change.doc.data() };
          data.showDelete = false;
          this.chatMessages.push(data);
        }
        if (change.type === 'removed') {
          const index = this.chatMessages.findIndex(message => message.id === change.doc.id);
          if (index !== -1) {
            this.chatMessages.splice(index, 1);
          }
        }
      });
    });
  },
};
  </script>
  
  <style scoped lang="scss">
  .chat-box {
    display: flex;
    background-color: #dcf8c6;
    flex-direction: column;
    height: 100%;
    overflow-y: auto;
    padding: 10px;
  }
  
  .chat-message {
    display: flex;
    align-items: flex-start;
    margin-bottom: 10px;
  }
  
  .message-bubble {
    background-color: #dcf8c6;
    border-top-left-radius: 20px; 
    border-bottom-left-radius: 20px;
    border-bottom-right-radius: 10px; 
    padding: 5px;
    max-width: 70%;
    word-wrap: break-word;
    margin-left: 10px;
  }
  
  .my-message .message-bubble {
    background-color: #128c7e;
    color: white;
    border-top-left-radius: 10px; 
    border-top-right-radius: 20px; 
    border-bottom-left-radius: 10px; 
    border-bottom-right-radius: 20px;
    margin-left: 0;
  }
  
  h1 {
    text-align: center;
    background-color: #075e54;
    color: white;
    padding: 10px;
    margin: 0;
  }
  
  input {
    width: 100%;
    padding: 10px;
    border: none;
    border-top: 1px solid #ccc;
    background-color: #f2f2f2;
    outline: none;
  }
  
  input:focus {
    background-color: #ffffff;
  }
  
  .send-button {
    background-color: #128c7e;
    color: white;
    border: none;
    border-radius: 50%;
    padding: 10px;
    cursor: pointer;
    margin-left: 10px;
  }
  
  .delete-button {
    color: rgb(20, 8, 1);
    cursor: pointer;
    margin-left: 5px;
  }
  </style>
   