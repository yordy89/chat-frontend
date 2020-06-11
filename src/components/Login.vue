<template>
    <v-card width="400" class="mx-auto mt-4">
        <v-card-title>Login</v-card-title>
        <v-card-text>
            <v-text-field type="email" label="Email" v-model="user.email"></v-text-field>
             <v-text-field type="password" label="Password" v-model="user.password"></v-text-field>
        </v-card-text>
        <v-card-actions>
            <v-btn color="success" block @click="entrar()">Entrar</v-btn>
        </v-card-actions>
        <div class="ml-3">
             <v-label>
            Si no tiene un usuario puede Registrarse 
            <router-link :to="{name:'Registro'}">Aqui</router-link>
        </v-label>
        </div>
        
    </v-card>
</template>



<script>
import axios from "axios";
import VueAxios from 'vue-axios'
import router from '../router'
import {mapMutations} from 'vuex'
export default {
    data(){
        return{
            user:{
                email:'',
                password:''
            }
        }
    },

    methods:{
        ...mapMutations(['modificarUseranme']),
       async entrar(){
         const usuario = await axios.post('/login',this.user)
         if(!usuario){
             console.log(usuario)
         }else{
           this.modificarUseranme(usuario.data.usuario)
           router.push({name:'Chat'})
         }
           }
    }
}
</script>