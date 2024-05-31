<template>
  <div id="app">
    <header>
      <img alt="Pokemon" src="./assets/International_Pokémon_logo.svg.png">
      <h2>¿Quién es ese Pokémon?</h2>
      <p>Pokemones descubiertos: <strong>{{ contador }}</strong></p>
    </header>
    <main class="container d-flex">
      <section class="row">
          <cardPokemon class="col-3 gap-4 my-4" v-for="(pokemon, index) in pokemones" :key="index" :nombrePoke="pokemon.name"
            :imgPoke="pokemon.image" @descubrir="mostrarPoke()"/>
      </section>
    </main>
  </div>
</template>

<script>
import cardPokemon from './components/cardPokemon.vue'
import axios from 'axios';
export default {
  name: 'App',
  components: {
    cardPokemon,
  },

  async mounted() {
    try {
      const getData = async function (url) {
        let response = await axios.get(url)
        return { image: response.data.sprites.other.dream_world.front_default, name: response.data.name }
      }
      let response = await axios.get("https://pokeapi.co/api/v2/pokemon/");
      let { data } = response;
      Promise.all(data.results.map(pokemon => getData(pokemon.url)))
        .then(pokemones => this.pokemones = pokemones)
        .catch(error => {
          alert('error en procesar pokemones')
          console.log(error)
        })

    } catch (error) {
      if (error.code == "ERR_NETWORK") {
        return alert("En estos momento el servidor está caído, puede intentar más tarde.")
      }
      if (error.response.status == 404) {
        alert("El recurso que está buscando no existe.");
      } else {
        alert("El servidor en estos momentos no puede procesar su solicitud.")
      }
    }
  },
  data() {
    return {
      pokemones: [],
      fotoPoke: [],
      contador: "0"
    }
  },
  methods:{
        mostrarPoke: function(){
            this.contador++
        }
    },
}
</script>

<style>
#app {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  text-align: center;
  color: #112d49;
  margin-top: 60px;
}

#app img {
  height: 150px;
  margin-bottom: 30px;
}

/* .cards {
} */
</style>


<!-- let urlIndice = "https://pokeapi.co/api/v2/pokemon";
let getPokemon = async (url) => {
    let response = await fetch(url);
    let data = await response.json();
    return {id: data.id, nombre: data.name, images: data.sprites.front_default}
}
let main = async () => {
    let response = await fetch(urlIndice);
    let data = await response.json();
    let arrayPromesas = data.results.map(pokemon => getPokemon(pokemon.url));
    Promise.all(arrayPromesas).then(pokemones => console.log(pokemones));
}
main(); -->