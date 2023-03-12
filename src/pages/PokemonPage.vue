<template>
    <h1 v-if="!pokemon">Espere por favor...</h1>

    <div v-else>
        <h1>Quien es este pokemon</h1>

        <!-- TODO: Picture -->
        <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon"/>
        <!-- TODO: Options -->
        <PokemonOptions :pokemons="pokemonArr" @selection="checkAnswer( $event )"/>

        <template v-if="showAnswer" >
            <h2 class="fade-in">
                {{ message }}</h2>
            <button 
            @click="newGame()">
              Nuevo Juego
            </button>
        </template>

    </div>

</template>

<script>
import PokemonPicture  from '@/components/PokemonPicture.vue'
import PokemonOptions  from '@/components/PokemonOptions.vue'

import getPokemonOptions from '@/helpers/getPokemonOptions'

export default {

    components: {
        PokemonPicture,
        PokemonOptions
    },
    data(){
      return {
        pokemonArr: [],
        pokemon:null,
        showPokemon: false,
        showAnswer: false,
        message: '',
      }
    },
    methods:{

      async mixPokemonArray(){
        this.pokemonArr = await getPokemonOptions()

        const rndInt = Math.floor( Math.random() * 4)
        this.pokemon = this.pokemonArr[ rndInt ]
      },
      checkAnswer( selectedId ){
        this.showPokemon = true
        this.showAnswer = true
        // const isCorrect = selectedId === this.pokemon.id ? this.showPokemon = true  : this.showPokemon = false
        
        if(selectedId === this.pokemon.id){
          this.message = `Correcto, ${ this.pokemon.name }`
        } else {
          this.message = `Oops, era ${ this.pokemon.name }`
        }

      },
      newGame(){
        this.showAnswer = false
        this.showPokemon = false
        this.pokemonArr = []
        this.mixPokemonArray()
        
      }
    },
    mounted(){
      this.mixPokemonArray()
    }

}
</script>
