<template lang="pug">
  .pokemonCard(@click="getInfo" :style="pokemonData ? '' : 'cursor: pointer'")
    p {{ pokemon.name }}
    img(:src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${index}.png`")
    template(v-if="pokemonData")
      p {{ this.pokemonData.flavorText.flavor_text }}
</template>

<script>
export default {
  data() {
    return {
      pokemonData: null
    }
  },
  props: ['pokemon', 'index'],
  created() {
    console.log(this.index)
  },
  methods: {
    getInfo() {
      fetch(`https://pokeapi.co/api/v2/pokemon-species/${this.index}/`)
      .then(res => res.json())
      .then(res => {
        // const flavorText = res.flavor_text_entries.find(entry => entry.language.name === 'en')
        // console.log(flavorText)
        this.pokemonData = {
          flavorText: res.flavor_text_entries.find(entry => entry.language.name === 'en')
        };
      })
    }
  }
}
</script>

<style scoped>
* {
  text-align: center
}
.pokemonCard {
  /* margin: 15px auto; */
  border-radius: 3px;
  border: 2px gray solid;
  padding: 20px;
}

.pokemonCard p:first-child {
  text-transform: capitalize;
}

</style>

