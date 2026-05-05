<script setup>
import { ref, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'

const route = useRoute()
const router = useRouter()

const pokemon = ref(null)

const fetchPokemon = async () => {
  try {
    const res = await fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.id}`)
    pokemon.value = await res.json()
  } catch (error) {
    console.error('Erro ao carregar Pokémon:', error)
  }
}

onMounted(fetchPokemon)
</script>

<template>
  <div v-if="pokemon" class="detail-container">

    <!-- BOTÃO VOLTAR -->
    <button class="back-btn" @click="router.push('/')">
      Voltar
    </button>

    <!-- CARD -->
    <div class="modal-content">

      <h2>{{ pokemon.name }}</h2>

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

      <p><strong>Altura:</strong> {{ pokemon.height }}</p>
      <p><strong>Peso:</strong> {{ pokemon.weight }}</p>

    </div>

  </div>

  <!-- fallback (caso demore carregar) -->
  <div v-else style="text-align:center; margin-top:50px;">
    Carregando Pokémon...
  </div>
</template>