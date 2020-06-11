<template>
  <v-app>
  <v-container>
    <router-view></router-view>
  </v-container>
  </v-app>
</template>

<script>
import Chat from '@/components/Chat'
import Registro from '@/components/Registro'

export default {
  name: 'App',

  components: {
    Chat,
    Registro
  },

  data: () => ({
  
  }),

 

  
 
  
};
</script>


