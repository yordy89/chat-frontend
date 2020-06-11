<template>
<v-content>
    <v-card width="400px" class="mx-auto mt-4">
        <v-card-title>Registro</v-card-title>
        <v-card-text>
        <v-text-field type="email" label="Email" v-model="user.email"  ></v-text-field>
         <v-text-field type="text" label="Usuario" v-model="user.usuario"  ></v-text-field>
        <v-text-field type="password" label="Password" v-model="user.password"  ></v-text-field>
        </v-card-text>
        <v-card-actions>
            <v-btn color="success" block @click="entrar()">Registrar</v-btn>
        </v-card-actions>
        <div class="ml-3">
              <v-label >
            Si ya tiene un usuario puede logearse 
            <router-link :to="{name:'Login'}">Aqui</router-link>
        </v-label>
        </div>
      
        
        
    </v-card>

</v-content>
</template>

<script>
 import axios from 'axios'
 import VueAxios from 'vue-axios'
 import router from '../router'
export default {
   
    data(){
        return{
          user: {
              email:'',
              password:'',
              usuario:''
          },
          userDefault: {
               email:'',
              password:'',
              usuario:''
              }
        }
    },
    methods:{
      async entrar(){
        const usuario = await axios.post('/registro',this.user)
        this.user = Object.assign({},this.userDefault)
        router.push({name:'Login'})
        }
    }
}
</script>