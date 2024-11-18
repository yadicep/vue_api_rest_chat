<template>
   <div class="chat-container">
      <!-- 4. Hacer uso del style y/o class binding para el direccionamiento y el color de fondo de los mensajes según corresponda a su propietario (:class="getMessageClass) -->
      <div v-for="(message, index) in messages" :key="index" :class="getMessageClass(message.owner)">
         <div class="message-name">{{ message.name }}</div>
         <!-- 4. Hacer uso del style y/o class binding para el direccionamiento y el color de fondo de los mensajes según corresponda a su propietario (:style="{ backgroundColor: message.color }) -->
         <div class="message-text" :style="{ backgroundColor: message.color }">
            <p class="m-0 p-0">{{ message.text }}</p>
         </div>
      </div>
   </div>
</template>
 
<script setup> 
   const props = defineProps({
      messages: Array,
      user1: Object,
      user2: Object
   });
   
   // Definidas para alinear el texto correctamente
   const getMessageClass = (owner) => {
      return owner === 1 ? 'text-start' : 'text-end';
   };
</script>
 
<style scoped>
   .chat-container {
      height: 515px;
      overflow-y: scroll;
      background-color: #f0f0f0;
      padding: 10px;
   }

   .text-start {
      text-align: left;
      margin-bottom: 8px;
      max-width: 90%;
      margin-left: 18px;
   }
   
   .text-end {
      text-align: right;
      margin-bottom: 8px;
      max-width: 90%;
      margin-left: auto;
   }
   
   .message-name {
      font-weight: normal;
      margin-bottom: 1px;
      font-size: 0.9em;
      color: #484848;
   }
   
   .message-text {
      padding: 5px;
      border-radius: 10px;
      font-size: 1.1em;
   }
</style>