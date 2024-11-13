<template>
    <div id="chats" class="minHeight hidden md:flex flex-col w-1/3 h-screen bg-[#edf6f9]">      
        <!-- Contenedor principal de Chat -->
        <div id="main-chat" class="flex w-full">      
          <div class="w-16 h-16 ml-5 mt-5 rounded-full" :style="code"></div>
          <div>
            <h2 class="ml-5 mt-5">General Chat</h2>
            <p class="ml-5 text-sm">{{connectedUsers}} Online</p>
          </div>     
        </div>
        
        <!-- Opciones de Chat -->
        <div id="chatOptions" class="flex mx-auto mt-7 w-[90%] h-12 border-b-2 ">
          <button @click="toggleView(true)" 
                  class="flex cb w-[50%] h-12 border-b-2 border-r-2 justify-center items-center" 
                  :class="{ 'active-button': count, 'inactive-button': !count }">
              <h3>Chats</h3>
          </button>
          <button @click="toggleView(false)" 
                  class="flex cb w-[50%] h-12 border-b-2  justify-center items-center"
                  :class="{ 'active-button': !count, 'inactive-button': count }">
            <h3>Recent</h3>
          </button>
        </div>
        
        <!-- Componente dinámico con flex-grow para tomar espacio restante -->
        <div class="flex-grow overflow-y-auto border-t-2 ">
          <component :is="currentComponent" class="h-full w-full flex flex-col" />
        </div>
      </div>
</template>


<script>

import { ref, computed, inject } from 'vue'
import ChatButtons from './chat-buttons.vue'
import recent from './recent.vue'
import { socketSymbol } from '../main'; 
import code from '../assets/code.jpg'

export default {
    name: 'Chats',
    components: {
      ChatButtons,
      recent,
    },
    setup(){
        const count = ref(true);
        const connectedUsers = ref(0);
        const socket = inject(socketSymbol);

        const toggleView = (showChats) => {
            count.value = showChats;
        };

        const currentComponent = computed(() => {
            return count.value ? 'ChatButtons' : 'recent';
        });

        socket.on('userConnected', ( users ) => {
            if ( users.length > 0 ) {
            connectedUsers.value = users.length;
            } else {
            connectedUsers.value = 0;
            }
        })

        socket.on('userDisconnected', ( users ) => {
            if ( users.length > 0 ) {
            connectedUsers.value = users.length;
            } else {
            connectedUsers.value = 0;
            }
        })

        return {
            connectedUsers,
            currentComponent,
            toggleView
        }
    },
    computed: {
        code() {
          return {
            backgroundImage: `url(${code})`,
            backgroundSize: 'cover', 
            backgroundPosition: 'center center' 
          };
        },
    },
}

</script>