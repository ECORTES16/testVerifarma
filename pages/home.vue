<template >
    <v-card class="fondo_principal">
      <v-data-iterator
        color=white
        v-if="list.length > 0"
        :items="list"
        :items-per-page="9"
        >
        <template v-slot:header>
          <v-toolbar class="px-2 fondo_card">
            <v-text-field
              @keyup="busqueda"
              v-model="search"
              density="comfortable"
              placeholder="Search"
              prepend-inner-icon="mdi-magnify"
              style="max-width: 300px;"
              variant="solo"
              hide-details
            ></v-text-field>
          </v-toolbar>
          <v-btn class="btn-logout" color="red" @click="logout()">Logout</v-btn>
        </template>
        <template  v-slot:default="{ items }">
          <v-container  class="pa-4" fluid>
            <v-row dense>
              <v-col
                v-for="item in items"
                :key="item.raw.data.Title"
                cols="auto"
                md="4"
              >
 
                <v-card class="pb-1 fondo_card" border flat v-if="item.raw.data.Response == 'True'">
  
                  <template v-slot:title>
                    <strong class="text-h6 mb-2 white">{{ item.raw.data.Title }}</strong>
                  </template>
                  <v-img
                    height="300"
                    :src="item.raw.data.Poster "
                    cover
                    ></v-img>
                    <v-row>
                      <v-col md="9">
                        <v-list-item class="text-medium-emphasis" >
                          <v-list-item-title>Type: {{item.raw.data.Type}}</v-list-item-title>
                          <v-list-item-title>Year: {{item.raw.data.Year}}</v-list-item-title>
                          <v-list-item-title>Genre: {{item.raw.data.Genre}}</v-list-item-title>
                        </v-list-item>
                      </v-col>
                        <v-col>
                          <v-btn
                          class="text-none mt-8"
                          color="white"
                          min-width="92"
                          variant="outlined"
                          rounded
                          @click="detalle(item.raw.data.imdbID)"
                          >
                          Detail
                        </v-btn>
                      </v-col>
                  </v-row>
                </v-card>
              </v-col>
            </v-row>
          </v-container>
        </template>
  
        <template v-slot:footer="{ page, pageCount, prevPage, nextPage }">
          <div class="d-flex align-center justify-center pa-4 text-pagination">
            <v-btn
              :disabled="page === 1"
              density="comfortable"
              icon="mdi-arrow-left"
              variant="tonal"
              rounded
              @click="prevPage"
            ></v-btn>
  
            <div class="mx-2 text-caption">
              Page {{ page }} of {{ pageCount }}
            </div>
  
            <v-btn
              :disabled="page >= pageCount"
              density="comfortable"
              icon="mdi-arrow-right"
              variant="tonal"
              rounded
              @click="nextPage"
            ></v-btn>
          </div>
        </template>
      </v-data-iterator>
    </v-card>
  
    <template>
    <div class="text-center pa-4">
  
      <v-dialog
        v-model="dialog"
        width="auto"
      >
      <v-card
      :disabled="loading"
      :loading="loading"
      class="mx-auto "
      max-width="70%"
    >
  
      <v-row class="ma-0">
        <v-col class="fondo_principal pa-0">
          <v-img
          height="600px"
          :src="movie_detail[0].data.Poster"
          
          ></v-img>
        </v-col>
        <v-col>
          <v-btn
          class="btn_close"
            icon="mdi-close"
            variant="text"
            @click="dialog = false"
          ></v-btn>
        <v-card-item>
          
          <v-card-title >{{ movie_detail[0].data.Title }}</v-card-title>
        </v-card-item> 
      <v-row
      align="center"
      class="mx-2"
      >      
        <v-rating
          :model-value=" movie_detail[0].data.imdbRating"
          color="amber"
          density="compact"
          size="large"
          length="1"
          half-increments
          readonly
          ></v-rating>
        
        <div class="text-grey ">
          {{movie_detail[0].data.imdbRating}}
        </div>
      </v-row>
  
        <v-row class="mx-1">
          <div class="text-grey ms-4">
            Director:    {{ movie_detail[0].data.Director }}
          </div>
        </v-row>
        <v-row class="mx-1">
          <div class="text-grey ms-4">
            Year:    {{movie_detail[0].data.Year}}
          </div>
        </v-row>
        <v-row class="mx-1">
          <div class="text-grey ms-4">
            Runtime:    {{movie_detail[0].data.Runtime}}
          </div>
        </v-row>
      
        <v-card-text>        
          <div class="my-4 text-subtitle-1">
          </div>        
          <div>{{movie_detail[0].data.Plot}}</div>
        </v-card-text>
        <v-card-text>        
          <v-chip class="ma-1">{{ movie_detail[0].data.Language }}</v-chip> 
          <v-chip class="ma-1">{{ movie_detail[0].data.Genre }}</v-chip> 
          <v-chip class="ma-1">{{ movie_detail[0].data.Type }}</v-chip> 
        </v-card-text>        
  
      </v-col>
      
    </v-row>
  </v-card>
    </v-dialog>
  </div>
  </template>
  
  </template>
  <style>
  .fondo_principal{
    min-height:100vh;
    background-color: rgb(0, 0, 0) !important;
  }
  .fondo_card{
    background-color: rgb(65 65 65 / 10%) !important;
  }
  .text-h6, .v-list-item-title, .text-pagination{
    color: white !important;
    font-weight: bolder;
  }
  .btn_detalle{
      position: absolute !important;
      right: 8px;
      bottom: 10px;
  }
  .btn_close{
    position: absolute !important;
    right: 4px;
    top: 0px;
  }
  .btn-logout{
    position: absolute !important;
    right: 4px;
    top: 10px;
  }
  
  </style>
  <script>
//   La variable search se utiliza para realizar la busqueda de pelicula por su nombre
//   La variable list contendra el listado de las peliculas a mostrar
//   El servicio de la pagina https://omdbapi.com/ no muestra una lista, por ello se usara movie_ids para realizar una busqueda y crear un listado de peliculas
//   La variable movie_detail se utiliza para guardar el detalle de la pelicula a mostrar
//   la variable dialog se utiliza para desplegar el dialog o modal que mostrara el detalle de la pelicula 
  import axios from "axios";
    export default {
      data: () => ({
        search: '',
        list: [],
        movie_ids:[
          'tt0120903','tt0103584','tt0120903','tt3385516','tt0458525','tt4574334','tt0813715','tt0944947','tt0903747',
          'tt0112442','tt0172156','tt1502397','tt1375666','tt0133093','tt0234215','tt0234215','tt10838180','tt0816692',
          'tt0088247','tt0103064','tt0438488','tt1340138','tt6450804','tt1392190','tt7286456','tt30476486','tt0137523'
        ],
        movie_detail:[],
        dialog: false,
      }),
      mounted(){
        // Se verifica si tiene una session activa o permitida para poder ver el contenido de la pagina en caso contrario se redirecciona al login
          if(window.localStorage.accesoPermitido == 'true'){
              this.getList()
          }else{
            return navigateTo('/')
          }
      },
      methods:{
        logout(){
            // Se cambia el valor en el localStorage de accesoPermitido para cerrar la session 
            window.localStorage.setItem('accesoPermitido', false);
            return navigateTo('/')

        },
        async busqueda(){
            // Busqueda de pelicula por su titulo
            try{
                if(this.search != ''){
                  const response = await axios({
                    method: 'GET',
                    url: 'http://www.omdbapi.com/?t='+this.search+'&plot=full&apikey=ce09d504' 
                  }) 
                    this.list=[response]  
                }else{
                  this.getList()
                }
            }catch(e){
                alert(e)
            }
        },
        async detalle(mId){
            // Se consulta el detalle de la pelicula usando el id
          this.movie_detail = await this.getMovieData([mId])
          this.dialog = true
        },
        async getList(){
            // Se crea la lista de peliculas basandose en los ids de la variable movie_ids
          let infoMovies = await this.getMovieData(this.movie_ids)
          this.list = infoMovies
        },
        async getMovieData(mIds){
            // se crea la lista de peliculas consultando sus ids
            let dataMovie = []
            try{
                for(const ids of mIds){
                const response = await axios({
                    method: 'GET',
                    url: 'http://www.omdbapi.com/?i='+ids+'&plot=full&apikey=ce09d504' 
                })            
                dataMovie.push(response)
                }
          }catch(e){
            alert(e)
          }
  
          return dataMovie 
  
        }  
  
      }
    }
  </script>