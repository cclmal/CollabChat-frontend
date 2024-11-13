<template>
      <div id="app" class="flex">
        <SideBar/>
        <Chats/>
        <Chat/>
    </div> 
</template>
  

<script>
import Chat from './components/chat.vue';
import Chats from './components/chats.vue';
import SideBar from './components/sidebar.vue';

export default {
    name: 'App',
    components: { 
      SideBar,
      Chat,
      Chats
    }
}

</script>

<style scoped>
  .sidebar-background {
    box-shadow: 0 5px 22px 0 rgba( 31, 38, 135, 0.38 );
    backdrop-filter: blur( 0px );
    -webkit-backdrop-filter: blur( 0px );
  }

  .active-button {
    border-bottom-color: #61a5c2;
    color: #61a5c2;
  }

  /* Estilos para cuando un botón está inactivo */
  .inactive-button {
    border-bottom-color: gray; /* o cualquier otro color que desees */
    color: gray; /* o cualquier otro color que desees */
  }

  .cb {
    transition: color 0.3s ease, border-color 0.3s ease;
  }
</style>
