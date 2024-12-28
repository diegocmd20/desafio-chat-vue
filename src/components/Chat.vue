<template>
    <div>
      <div
        v-for="(message, index) in messages"
        :key="index"
        class="message"
        :class="{ 'current-user': message.isCurrentUser }"
        :style="{ backgroundColor: message.color }"
      >
        <strong>{{ message.user }}:</strong>
        <span>{{ message.text }}</span>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "Chat",
    props: {
      messages: {
        type: Array,
        required: true,
      },
    },
  };
  </script>
  
  <style scoped>
  .message {
    padding: 10px;
    margin: 5px 0;
    border-radius: 5px;
    color: #000;
  }
  
  .message.current-user {
    text-align: left;
  }
  
  .message:not(.current-user) {
    text-align: right;
  }
  </style>
  