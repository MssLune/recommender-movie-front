// Todo esto de template es solo html
<template>
 <v-container>
  <v-container>
    
    <h1 class="titulos">¡Encuentra tu tema de interes!</h1>
    <span class="subtitulo">Realizar busqueda por codigo de pelicula</span>
    <!-- <h1>Busca tu película</h1> -->
    <v-divider></v-divider>
    <v-form>
      <v-container>
        <v-row>
          <v-col cols="8">
            <!-- Este es el campo de texto y recibiremos acá el id que se guarda en la variable movieID (línea 45) como string -->
            <v-text-field
              v-model="movieID"
              label="Ingresa el ID de una película"
            ></v-text-field>
            <!-- Este es el botón que va a ejecutar la función search (linea 52) que en realidad accionará axios para traer la data del API -->
            <v-btn class="tfbusqueda" style="border-radius:120px;" @click="search">Buscar</v-btn>
          </v-col>
        </v-row>
        <v-row>    
          <hr>
          <ul class="mt">
          <!-- acá hago un for con v-for en vez de un foreach, hago una iteración en names, cada elemento lo llamo name y lo de :key
          es un identificador único que es necesario para que vue no se confunda -->
             <v-card class="iftitle" style="background-color:rgb(255, 255, 255, 6%);border-radius: 40px; box-shadow: 0px 0px rgba(0, 0, 0, 0);" v-if="this.sour.title!=null">
               <v-card-text>
                 <span class="subtitulo">Pelicula Buscada</span><br/>
                  <h1 class="h1card">{{this.sour.title}}</h1>
                 </v-card-text>
               <v-card-text>
                 <span class="subtitulo">Código de Pelicula</span><br/>
                 <h1 class="h1cardid">{{this.sour.id}}</h1></v-card-text>
             </v-card>
              <!-- PELICULA BUSCADA: {{this.sour.title}}<br/>PELICULA ID: {{this.sour.id}} <br/>PELICULAS RECOMENDADAS -->
            <span class="iftitle" v-else></span>
            
            <v-layout row class="mt">
                <v-flex>
                <li v-for="name in names" :key="name.id">
                  <v-card class="cardf">
                        <v-card-title primary-title>
                          <div>
                          <div class="headliner">{{name.title}}</div>
                          <h4 class="grey--textb">Genero</h4> 
                          <span class="grey--text">{{name.genres}}</span><br/>
                          <h4 class="grey--textb">Codigo</h4> 
                          <span class="grey--text">{{name.id}}</span>
                          </div>
                        </v-card-title>

                        
                  </v-card>  

                </li>
                </v-flex>
            </v-layout>

          </ul>
        </v-row>
      </v-container>
    </v-form>
  </v-container>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "Home",

  // en esta sección se hacen las declaraciones de las variable que luego se usan arriba en la parte html(template)
  data() {
    return {
      // acá solo declaro las dos variables, el primero para el ID que usamos para el api y el segundo va a guardar lo respuesta
      movieID: "",
      names: [],
      sour:[],
    };
  },
  methods: {

    // esta es la función que se acciona cuando hago click en el botón, mira la línea 16 en donde dice @click
    search() {

      // acá primero convierto el id que estaba en string(línea 45) a int porque así lo pide el api
      var page = parseInt(this.movieID); //cuando nos referimos a los datos de data(), hay que usar this.

      // luego paso ese id al final del url
      axios.post("http://192.168.1.9:9090/api/v1/movies/recommeder",{
        movieId: page,
        ntop:5
      })
      .then((res) => {
        console.log(res.data);
        console.log(res.data.source);
        // acá guardo la respuesta en la variable que definí en data(), ve la línea 46
        this.names = res.data.recommerders;
        this.sour = res.data.source;
      });
    },
  },
};
</script>
