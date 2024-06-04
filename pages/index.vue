<template>
    <v-sheet class="bg-black pa-12" height="100vh" rounded>
      <v-card class="mx-auto px-6 py-8" max-width="344">
        <v-form
          v-model="form"
          @submit.prevent="onSubmit"
        >
          <v-text-field
            v-model="user"
            :readonly="loading"
            :rules="[required]"
            class="mb-2"
            label="User"
            clearable
          ></v-text-field>
  
          <v-text-field
            v-model="password"
            :readonly="loading"
            :rules="[required]"
            label="Password"
            type="password"
            placeholder="Enter your password"
            clearable
          ></v-text-field>
  
          <br>
  
          <v-btn
            :disabled="!form"
            :loading="loading"
            color="success"
            size="large"
            type="submit"
            variant="elevated"
            block
          >
            Sign In
          </v-btn>
        </v-form>
      </v-card>
    </v-sheet>
    <v-dialog v-model="dialog" max-width="500">
        <template v-slot:default="{ isActive }">
            <v-card title="">
            <v-card-text>
                {{msj}}
            </v-card-text>

            <v-card-actions>
                <v-spacer></v-spacer>

                <v-btn
                text="Close Dialog"
                @click="isActive.value = false"
                ></v-btn>
            </v-card-actions>
            </v-card>
        </template>
    </v-dialog>
  </template>
  <script>
    import axios from "axios";

    export default {
      data: () => ({
        form: false,
        user: null,
        password: null,
        loading: false,
        dialog: false,

      }),
      created(){

      },
  
      methods: {
       async onSubmit () {
            try{
                // para el login se realizo una logica para aprovechar la consulta de alguna pelicula, entonces como user se utilizara el 
                // titulo de la pelicula y para el password el imdbID de la pelicula 

                if (!this.form) return
                this.loading = true
                const response = await axios({
                    method: 'GET',
                    url: 'http://www.omdbapi.com/?t='+this.user+'&plot=full&apikey=ce09d504' 
                }) 
                if(response.data.Response == 'True'){
                    if(this.password == response.data.imdbID){
                        window.localStorage.setItem('accesoPermitido', true);
                        return navigateTo('/home')

                    }else{
                        this.dialog = true
                        this.msj = 'Datos Incorrectos'
                    }
                }else{
                    this.dialog = true
                    this.msj = 'El usuario no existe'
                }
                setTimeout(() => (this.loading = false), 2000)
            }catch(error){
                alert(error)
            }
  
        },
        required (v) {
          return !!v || 'Field is required'
        },
      },
    }
  </script>