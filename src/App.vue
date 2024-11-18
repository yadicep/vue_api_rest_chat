<template>
   <div class="row">
      <!-- Usuario 1 -->
      <div class="col-3" v-if="user1">
         <div class="border border-secondary-subtle border-4 rounded-5"><div class="border border-dark border-3 rounded-5"><img :src="user1.picture.large" alt="Usuario 1" class="img-fluid border border-white border-4 rounded-5 w-100" /></div></div>
         <h4>{{ user1FullName }}</h4><br>
         <input type="color" v-model="user1Color" class="form-control w-100"/>
         <textarea v-model="user1Message" placeholder="Escribe tu mensaje"></textarea>
         <button @click="sendMessage(1)" type="button" class="btn btn-dark">Enviar</button>
      </div>
      <!-- Chat -->
      <div class="col-6">
         <Chat :messages="messages" :user1="user1" :user2="user2" />
      </div>
      <!-- Usuario 2 -->
      <div class="col-3" v-if="user2">
         <div class="border border-secondary-subtle border-4 rounded-5"><div class="border border-dark border-3 rounded-5"><img :src="user2.picture.large" alt="Usuario 2" class="img-fluid border border-white border-4 rounded-5 w-100" /></div></div>
         <h4>{{ user2FullName }}</h4><br>
         <input type="color" v-model="user2Color" class="form-control w-100"/>
         <textarea v-model="user2Message" placeholder="Escribe tu mensaje"></textarea>
         <button @click="sendMessage(2)" type="button" class="btn btn-dark">Enviar</button>
      </div>
   </div>
   <!-- 3. Utilizar los eventos para agregar las interacciones que permitan el registro de nuevos mensajes en el chat: @click que dispara la función sendMessage (en los buttons) -->
</template>
 
<script setup>
   import { ref, onMounted, computed } from 'vue';
   import axios from 'axios';
   import Chat from './components/Chat.vue';
   
   // Variables reactivas
   const user1 = ref(null);
   const user2 = ref(null);
   const user1Message = ref('');
   const user2Message = ref('');
   const user1Color = ref('#ffcccb');
   const user2Color = ref('#add8e6');
   const messages = ref([]);
   
   // Propiedad computada para el nombre completo del usuario 1
   const user1FullName = computed(() => {
      return user1.value ? `${user1.value.name.first} ${user1.value.name.last}` : '';
   });
   
   // Propiedad computada para el nombre completo del usuario 2
   const user2FullName = computed(() => {
      return user2.value ? `${user2.value.name.first} ${user2.value.name.last}` : '';
   });
   
   // 1. Utilizar Axios para el consumo de datos de la API Random User
   const getRandomUsers = async () => {
      try {
         const response = await axios.get('https://randomuser.me/api/?results=2');
         user1.value = response.data.results[0];
         user2.value = response.data.results[1];
      } catch (error) {
         console.error('Error fetching users:', error);
      }
   };
   
   // Enviar mensajes
   const sendMessage = (user) => {
      if (user === 1 && user1Message.value.trim() !== '') {
         messages.value.push({
            name: `${user1FullName.value}`,
            text: user1Message.value,
            color: user1Color.value,
            owner: 1
         });
         user1Message.value = '';
      } else if (user === 2 && user2Message.value.trim() !== '') {
         messages.value.push({
            name: `${user2FullName.value}`,
            text: user2Message.value,
            color: user2Color.value,
            owner: 2
         });
         user2Message.value = '';
      }
   };
   
   // 2. Usar el ciclo de vida de los componentes para obtener la información de los usuarios al cargar la aplicación
   onMounted(() => {
      getRandomUsers();
   });
</script>
 
<style scoped>
   textarea {
      width: 100%;
      height: 80px;
      margin-top: 10px;
      margin-bottom: 10px;
   }
   
   button {
      display: block;
      width: 100%;
   }
</style>