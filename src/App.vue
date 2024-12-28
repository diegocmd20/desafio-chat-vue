<script>
import axios from "axios";
import Chat from "./components/Chat.vue";

export default {
  name: "App",
  components: { Chat },
  data() {
    return {
      users: [], // Usuarios obtenidos de la API
      messages: [], // Mensajes en el chat
      newMessages: ["", ""], // Nuevos mensajes por usuario
      messageColors: ["#d1e7ff", "#e2ffe2"], // Colores predeterminados de mensajes
    };
  },
  methods: {
    async fetchUsers() {
      try {
        const response = await axios.get("https://randomuser.me/api/?results=2");
        this.users = response.data.results.map((user) => ({
          name: `${user.name.first} ${user.name.last}`,
          picture: user.picture.large,
        }));
      } catch (error) {
        console.error("Error al obtener usuarios:", error);
      }
    },
    sendMessage(userIndex) {
      if (!this.newMessages[userIndex]) return;

      // Crear mensaje con color dinámico
      this.messages.push({
        user: this.users[userIndex].name,
        text: this.newMessages[userIndex],
        color: this.messageColors[userIndex], // Agregar el color del usuario
        isCurrentUser: userIndex === 0,
      });

      // Limpiar el campo de texto
      this.newMessages[userIndex] = "";
    },
  },
  mounted() {
    this.fetchUsers(); // Cargar usuarios al montar el componente
  },
};
</script>

<template>
  <div id="app">
    <div class="chat-container">
      <!-- Usuario 1 -->
      <div class="user">
        <img :src="users[0]?.picture" :alt="users[0]?.name" v-if="users.length > 0" />
        <h3>{{ users[0]?.name }}</h3>
        <input
          type="color"
          v-model="messageColors[0]"
          title="Selecciona un color"
        />
        <textarea
          v-model="newMessages[0]"
          placeholder="Escribe tu mensaje"
        ></textarea>
        <button @click="sendMessage(0)">Enviar</button>
      </div>

      <!-- Chat principal -->
      <div class="chat">
        <Chat :messages="messages" />
      </div>

      <!-- Usuario 2 -->
      <div class="user">
        <img :src="users[1]?.picture" :alt="users[1]?.name" v-if="users.length > 0" />
        <h3>{{ users[1]?.name }}</h3>
        <input
          type="color"
          v-model="messageColors[1]"
          title="Selecciona un color"
        />
        <textarea
          v-model="newMessages[1]"
          placeholder="Escribe tu mensaje"
        ></textarea>
        <button @click="sendMessage(1)">Enviar</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
#app {
  font-family: Arial, sans-serif;
  margin: 20px;
}

.chat-container {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
}

.user {
  width: 20%;
  text-align: center;
}

.user img {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  margin-bottom: 10px;
}

.user h3 {
  font-size: 18px;
  margin-bottom: 10px;
}

.user textarea {
  width: 100%;
  height: 100px;
  margin-bottom: 10px;
}

.user button {
  width: 100%;
}

.chat {
  width: 55%;
  border: 1px solid #ccc;
  padding: 10px;
  height: 400px;
  overflow-y: scroll;
  background-color: #f9f9f9;
}
</style>
