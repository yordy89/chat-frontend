<template>
     <v-content>
    
   <v-card width="600px" class="mx-auto mt-4">
     <v-card-title>Chat</v-card-title>
     <v-card-subtitle>
       Usuario: {{username}}
     </v-card-subtitle>
       <v-card-subtitle>
         <v-row>
           <v-col md="8">
             Online: {{users.length}}
           </v-col>
           <v-col md="4">
             <span v-if="typing">{{userTyping}} esta escribiendo</span>
           </v-col>
         </v-row>    
     </v-card-subtitle>
     <v-divider></v-divider>
     <v-card-text>
       
       <p v-for="(mensaje,index) in messages" :key="index">
        {{mensaje.username}} : {{mensaje.msg}}
       </p>
     </v-card-text>
     <v-card-actions>
       
       <v-text-field placeholder="Escriba su mensaje" v-model="mensaje"></v-text-field>
       <v-btn color="warning" small @click="enviar()">
          Enviar
       </v-btn>
       
     </v-card-actions>
     </v-card>  

    </v-content>
</template>


<script>
import {mapstate, mapState} from 'vuex'
import io from 'socket.io-client'

export default {
   data(){
       return{
    username:'',
    socket: io('http://localhost:5000'),
    messages:[],
    mensaje:'',
    users:[],
    typing:false,
    userTyping: ''
       }
   },
     watch:{
     mensaje(value){
       value ? this.socket.emit('typing',this.username) : this.socket.emit('stopTyping')
     }
   },
   computed:{
     ...mapState(['user'])
   },
   methods:{
   joinServe(){
     this.socket.emit('newUser',this.username)
    this.listen()
   },

   listen(){
    this.socket.on('userOnline', data => {
      this.users = data.usuarios
      this.messages = data.mensajes
        console.log(data.usuarios)
    })

     this.socket.on('salio', usuario => {
      this.users.splice(this.users.indexOf(usuario),1)
    })

     this.socket.on('msg', msg => {
      this.messages.push(msg)
    })

    this.socket.on('typing', username => {
      this.userTyping = username
      this.typing = true
    })
    this.socket.on('stopTyping',()=>{
      this.userTyping = 'username'
      this.typing = false
    })
  
    console.log(this.users)
   },
   enviar(){
     this.socket.emit('msg',this.mensaje)
     this.mensaje = ''
   },

  },
   mounted(){
     this.username = this.user
    if(!this.username) this.username = "Anonimo"
    this.joinServe()
  }
}
</script>

<style scope>
span{
  font-style: italic;
  color: darkorchid;
}
</style>