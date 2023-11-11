<template>
    <div class="flex flex-col w-full h-full p-4 rounded-2xl shadow-2xl chat-background" >
      <div class="flex-grow overflow-y-auto mb-4">
        <ul>
            <li v-for="message in messages" :key="message.id" :class="message.sentBy === currentUser.id ? 'flex justify-end' : 'flex justify-start mb-4'">
                <div v-if="message.sentBy !== currentUser.id" class="w-6 h-6 mx-2 bg-gray-400 rounded-full" :style="kitty(message)"></div> <!-- Círculo a la izquierda para mensajes recibidos -->
                  
                
                <div class="flex flex-col w-[30%]" :class="message.sentBy === currentUser.id ? 'items-end' : 'items-start'">
                  <span class="text-xs text-gray-500">{{ message.sentBy === currentUser.id ? 'You' : message.username }}</span>
                    <span 
                        class="max-w-[70%] py-2 px-4 my-1 rounded-lg" 
                        :class="{
                            'bg-blue-500 text-white': message.sentBy === currentUser.id,
                            'bg-[#edf2f4]': message.sentBy !== currentUser.id
                        }"
                    >
                        {{ message.text }}
                    </span>
                </div>

                <div v-if="message.sentBy === currentUser.id"  class="w-6 h-6 mx-2 bg-gray-400 rounded-full" :style="doggy(message)"></div> <!-- Círculo a la derecha para mensajes enviados por mí -->
            </li>
        </ul>     
      </div>
  
      <div class="flex h-14 border-t bg-[#edf6f9] border-gray-300 border-2  rounded-2xl">

          <input class="flex-grow px-4 py-2 outline-none  rounded-2xl" placeholder="Type..." v-model="newMessage" @keyup.enter="sendMessage">
          
          <button class="w-10 rounded-r-lg focus:outline-none" @click="sendMessage">
            <div class="mt-1 ">
                <Icon  size="20" color="black"><Link/></Icon>
            </div>      
          </button>

          <button class="w-10 rounded-r-lg focus:outline-none" @click="sendMessage">
            <div class="mt-1">
                <Icon  size="20" color="black"><EmojiEmotionsOutlined/></Icon>
            </div>      
          </button>

          <button class="w-10 mr-2 rounded-r-lg focus:outline-none" @click="sendMessage">
            <div class="mt-1">
                <Icon  size="20" color="black"><SendAlt/></Icon>
            </div>      
          </button>

      </div>
    </div>
  </template>
  
  <script>
    import { ref, inject } from 'vue';
    import { socketSymbol } from '../main'; 
    import { Icon } from '@vicons/utils';
    import { SendAlt, Link } from '@vicons/carbon'
    import { EmojiEmotionsOutlined, AddCircleOutlineFilled } from '@vicons/material'
  
  export default {
    name: 'Chat',
    components: {
      Icon,
      SendAlt,
      Link,
      EmojiEmotionsOutlined,
      AddCircleOutlineFilled
    },
    setup() {
      const currentUser = ref(null);
      const socket = inject(socketSymbol);
      const messages = ref([]);
      const newMessage = ref('');



      socket.on('connect', () => {
        const inputUsername = prompt("Enter a username:");
        if (inputUsername) {
          socket.emit('login', inputUsername);
        } else {
          alert('You must enter a username');
          window.location.reload();
        }
      });


      socket.on('usernameTaken', (msg) => {
        alert(msg);
        const inputUsername = prompt("Enter a username:");
        if (inputUsername) {
          socket.emit('login', inputUsername);
        } else {
          alert('You must enter a username');
          window.location.reload();
        }
      });


      socket.on('userConnected', ( users ) => {
        const userObj = users.find(user => user.id === socket.id);
        if (userObj) {
            currentUser.value = userObj;
        }
      })

      socket.on('chat message', (msg) => {
            messages.value.push(msg);
    });
  
      const sendMessage = () => {
            if (newMessage.value.trim() !== '') {
                const message = {
                    id: messages.value.length + 1,
                    text: newMessage.value,
                    color: currentUser._rawValue.color,
                    username: currentUser._rawValue.username,
                    sentBy: currentUser._rawValue.id
                };

                messages.value.push(message);
                socket.emit('chat message', message);
                newMessage.value = '';
            }
        };
  


      return {
        messages,
        newMessage,
        sendMessage,
        currentUser,
      };
    },
    computed: {
      kitty() {
        return (message) => {
          return {
            backgroundColor: message.color,
          }
        }
      },
      doggy() {
        return (message) => {
          return {
            backgroundColor: message.color,
          }
        }
      }
    }
  };
  </script>
  
  <style scoped>
  

  .chat-background {
    box-shadow: 0 5px 22px 0 rgba( 31, 38, 135, 0.38 );
    backdrop-filter: blur( 0px );
    -webkit-backdrop-filter: blur( 0px );
  }

  .message-container {
    max-height: 40px; /* ajusta esto según el tamaño de fuente y el padding deseado */
    display: flex;
    align-items: center; /* Esto centrará el contenido verticalmente */
    justify-content: flex-start; /* O flex-end para mensajes enviados por el usuario */
  }
  
  /* Ajustes en la burbuja del mensaje */
  .message-bubble {
    padding: 8px 12px; /* ajusta esto según tus necesidades */
    line-height: 1.4; /* Esto debería ser suficiente para la mayoría de los tamaños de texto */
    /* ...otros estilos... */
  }

  </style>