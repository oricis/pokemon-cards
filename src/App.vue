<template lang="pug">
  div
    h1.title Pokemon Cards!
    .list
      template(v-for="item in items")
        PokemonCard(:pokemon="item" :key="index" :index="item.index")
</template>

<script>
import PokemonCard from './components/pokemonCard.vue';
export default {
  components: {'PokemonCard': PokemonCard},
  data() {
    return {
      bottom: false,
      items: [],
      next: 'initial'
    }
  },
  created() {
    window.addEventListener('scroll', () => {
      const scrollY = window.scrollY;
      const visibleHeight = window.innerHeight;
      const pageLength = document.documentElement.scrollHeight;
      // console.log({visibleHeight, scrollY, pageLength})
      if (scrollY + visibleHeight >= Math.floor(pageLength * .95) || scrollY + visibleHeight >= pageLength) {
        this.bottom = true;
      } else {
        this.bottom = false;
      }
    })
    this.addPokemon();
  },
  watch: {
    bottom(bottom) {
      if (bottom) {
        this.addPokemon();
      }
    }
  },
  methods: {
    addPokemon() {
      // console.log('adding pokemon')
      if (this.next !== 'initial' && this.next) {
        fetch(this.next)
        .then(res => res.json())
        .then(res => {
          this.items = [
            ...this.items,
            ...res.results.map(pokemon => {
              pokemon.index = pokemon.url.slice(34, -1);
              return pokemon
            })
          ];
          this.next = res.next;
        })
        .catch(err => console.log(err))
      } else if(this.next === "initial") {
        fetch('https://pokeapi.co/api/v2/pokemon/?offset=0&limit=30')
        .then(res => res.json())
        .then(res => {
          console.log(res)
          this.items = [
            ...this.items,
            ...res.results.map(pokemon => {
              pokemon.index = pokemon.url.slice(34, -1);
              return pokemon
            })
          ];
          this.next = res.next;
        })
        .catch(err => console.log(err))
      }
    }
  }
}
</script>

<style>
.list {
  max-width: 1200px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  grid-gap: 10px;
  margin: auto;
}

* {
  font-family: sans-serif;
}

h1.title {
  text-align: center;
}

</style>
