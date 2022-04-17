<template>
  <!-- TODO: Images -->
  <p v-if="!pokemon">Espere por favor...</p>
  <template v-else>
    <div>¿Quien es este Pokémon?</div>
    <PokemonPicture
      :pokemonId="pokemon.id"
      :showPokemon="showPokemon"
    />
    <PokemonOptions
      :pokemons="pokemonArr"
      @selection-pokemon="checkAnswer"
    />
    <p>
      {{ message }}
    </p>
    <button v-if="message" @click="newGame">
      NUEVO JUEGO
    </button>
  </template>
  <!-- TODO: Opciones -->
</template>

<script>
import PokemonOptions from "@/components/PokemonOptions.vue"
import PokemonPicture from "@/components/PokemonPicture.vue"
import getPokemonOptions from "@/helpers/getPokemonOptions"

console.log(getPokemonOptions())

export default {
  name: "PokemonPage",
  components: {
    PokemonPicture,
    PokemonOptions,
  },
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      message: null,
    }
  },

  methods: {
    async mixPokemonArray() {
      this.pokemonArr = await getPokemonOptions()
      const randomInt = Math.floor(Math.random() * 4)
      this.pokemon = this.pokemonArr[randomInt]
    },
    checkAnswer(pokemonId) {
      this.showPokemon = true
      if (pokemonId === this.pokemon.id) {
        this.message = `Correcto, era ${this.pokemon.name}`
      } else {
        this.message = `Opps, era ${this.pokemon.name}`
      }
    },
    async newGame() {
      this.showPokemon = false
      this.message = null
      this.pokemon = null
      await this.mixPokemonArray()
    },
  },
  mounted() {
    this.mixPokemonArray()
  },
}
</script>

<style>
button {
  background-color: white;
  border-radius: 5px;
  cursor: pointer;
  width: 250px;
  padding: 10px 0;
  margin-top: 10px;
}
button:hover {
  background-color: rgba(0, 0, 0, 0.05);
}
</style>
