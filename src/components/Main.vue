<template>
  <div>
    <img class="logo" src="../assets/1280px-International_Pokémon_logo.svg.png" alt="">
    <h1>Pokeguia</h1>
    <h5>Busca el nombre de tu pokemon!</h5>
    
    <div class="search-bar">
      <input type="text" v-model="searchedPokemon" placeholder="Pikachu">
      <button @click="searchPokemon()">Buscar</button>
    </div>
    
    <div class="results">
      <img :src="this.pokemon.img" alt="">
      <h3>{{this.pokemon.name}}</h3>
      <Stat title="Habilidades" :stats="abilities"/>
      <Stat title="movimientos" :stats="moves"/>
    </div>

  </div>
</template>

<script>

  import Stat from "./Stats.vue"

  export default {
    
    name: "Main",
    data: ()=>({

      searchedPokemon: "",
      pokemon: {
        name: "",
        img: "",
        abilities: [],
        moves: []
      }

    }),
    components:{
      Stat,
    },
    props: {

    },

    methods: {
      async searchPokemon(){
        try{
          let res = await fetch(`https://pokeapi.co/api/v2/pokemon/${this.searchedPokemon}`);
          let data = await res.json();
          let {name, abilities, moves, sprites: {front_default : img}} = data;
          this.pokemon.name = name;
          this.pokemon.abilities = abilities;
          this.pokemon.moves = moves;
          this.pokemon.img = img;
        }
        catch(e){
          console.log(`Error: Pokemon ${this.searchedPokemon} no ha sido encontrado.`)
          this.pokemon.name = "Pokemon no encontrado!";
          this.pokemon.abilities = [];
          this.pokemon.moves =  [];
          this.pokemon.img = "";
        }
      }
    },
    created(){
      this.searchedPokemon = "pikachu"
      this.searchPokemon();
    },

    computed:{

      name(){
        return this.pokemon.name || "no name"
      },
      abilities(){
   
        let pokemonAbilities = this.pokemon.abilities.map(ability =>{
          return ability.ability.name || "";
        })
        return pokemonAbilities
      },
      moves(){
        let pokemonMoves = this.pokemon.moves.map(move =>{
          return move.move.name || "";
        })
        return pokemonMoves
      },
      pokemonImg(){
        return this.pokemon.sprites.front_default || "";
      }
    }
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .logo{
    width: 500px;
  }
  input{
    margin-right: 0.5em;
  }

  .results{
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .results img{
    width: 200px;
    padding-top: -100px;
  }
</style>
