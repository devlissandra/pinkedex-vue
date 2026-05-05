<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'

const pokemons = ref([])
const search = ref('')
const router = useRouter()

const fetchPokemons = async () => {
  const res = await fetch("https://pokeapi.co/api/v2/pokemon?limit=107&offset=386")
  const data = await res.json()

  const promises = data.results.map(p => fetch(p.url).then(r => r.json()))
  pokemons.value = await Promise.all(promises)
}

onMounted(fetchPokemons)

const goToPokemon = (id) => {
  router.push(`/pokemon/${id}`)
}
</script>

<template>
  <div>
    <div class="header">
      <input v-model="search" id="search" placeholder="Buscar Pokémon..." />

      <h1 class="title">
        <span class="pink">Pink</span><span class="dex">edéx</span>
      </h1>
    </div>

    <div id="pokedex">
      <div
        v-for="pokemon in pokemons.filter(p => p.name.includes(search.toLowerCase()))"
        :key="pokemon.id"
        class="card"
        @click="goToPokemon(pokemon.id)"
      >
        <h3>{{ pokemon.name }}</h3>
        <img :src="pokemon.sprites.front_default" />

        <div>
          <span
            v-for="t in pokemon.types"
            :key="t.type.name"
            class="type"
            :class="t.type.name"
          >
            {{ t.type.name }}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>