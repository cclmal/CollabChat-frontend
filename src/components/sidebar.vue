<template>
    <div id="sidebar" class="minHeight hidden md:flex flex-col w-36 min-w-[130px] h-screen bg-[#edf6f9] sidebar-background ">         
        <!-- Contenedor del logo con altura fija -->
        <div id="logo" :style="logoStyle" class="w-full h-24"></div>
        
        <!-- Contenedor que ocupará el espacio restante sin desbordarse -->
        <div id="desbordamiento" class="flex flex-col flex-grow  overflow-y-auto">
          
          <!-- Contenedor superior con íconos -->
          <div class="flex flex-col w-full items-center">
            <button class="mb-6 mr-1 mt-5" @mouseover="setHovered('chat')" @mouseout="clearHovered">
              <Icon size="36" :color="hoveredIcon === 'chat' ? '#61a5c2' : 'black'"> <ChatMultiple20Regular/> </Icon>
            </button>
            <button class="mb-5" @mouseover="setHovered('group')" @mouseout="clearHovered">
              <Icon size="34" :color="hoveredIcon === 'group' ? '#61a5c2' : 'black'"> <GroupsTwotone/> </Icon>
            </button>
            <button class="mb-5" @mouseover="setHovered('bookmark')" @mouseout="clearHovered">
              <Icon size="34" :color="hoveredIcon === 'bookmark' ? '#61a5c2' : 'black'"> <BookmarkMultiple28Regular/> </Icon>
            </button>
            <button class="mb-5" @mouseover="setHovered('more')" @mouseout="clearHovered">
              <Icon size="34" :color="hoveredIcon === 'more' ? '#61a5c2' : 'black'"> <Add/> </Icon>
            </button>
          </div>
    
          <!-- Contenedor inferior con íconos -->
          <div class="flex flex-col w-full justify-end items-center mt-auto">
            <button class="mb-6 mr-1"  @mouseover="setHovered('config')" @mouseout="clearHovered">
              <Icon size="32" :color="hoveredIcon === 'config' ? '#61a5c2' : 'black'"> <CloudSatelliteConfig/> </Icon>
            </button>
            <button class="mb-5"  @mouseover="setHovered('profile')" @mouseout="clearHovered">
              <Icon size="34" :color="hoveredIcon === 'profile' ? '#61a5c2' : 'black'"> <UserProfileAlt/> </Icon>
            </button>
          </div>
          
        </div>
        
      </div>
</template>

<script>
import { ref } from 'vue'
import { Icon } from '@vicons/utils'
import logo from '../assets/logo.png'
import { ChatBot, UserProfile, UserProfileAlt, CloudSatelliteConfig, Add } from '@vicons/carbon'
import {  AddCircleOutlineFilled, GroupsTwotone, GroupAddFilled } from '@vicons/material'
import { ChatMultiple20Regular, BookmarkMultiple28Regular } from '@vicons/fluent'

export default {
    name: 'SideBar',
    components: { 
      Icon, 
      ChatBot,
      AddCircleOutlineFilled,
      UserProfile,
      UserProfileAlt,
      CloudSatelliteConfig,
      ChatMultiple20Regular,
      GroupsTwotone,
      GroupAddFilled,
      Add,
      BookmarkMultiple28Regular
    },

    setup(){
        const hoveredIcon = ref(null);

        const setHovered = (iconName) => {
            hoveredIcon.value = iconName;
        };

        const clearHovered = () => {
            hoveredIcon.value = null;
        };

        return {
            hoveredIcon,
            setHovered,
            clearHovered
        };
    },
    computed: {
        logoStyle() {
          return {
            backgroundImage: `url(${logo})`,
            backgroundSize: 'cover', 
            backgroundPosition: 'center center' 
          };
        },
    },
}
</script>

<style lang="">
    
</style>