<template>
  <div class="max-w-6xl mx-auto mt-8 p-4">
    <h1 class="text-3xl font-bold mb-6">Pokédex</h1>

    <!-- loading / error -->
    <div v-if="loading" class="p-3 text-gray-600">Chargement liste...</div>
    <div v-else-if="error" class="p-3 text-red-600">Erreur: {{ error }}</div>

    <div v-else class="grid grid-cols-1 lg:grid-cols-4 gap-6">
      <!-- pokemon list -->
      <div class="lg:col-span-1 bg-gray-50 p-4 rounded-lg border">
        <div class="mb-4">
          <input
            v-model="searchQuery"
            type="text"
            placeholder="Rechercher..."
            class="w-full px-3 py-2 border rounded text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"
          />
        </div>
        <h2 class="font-bold mb-3">Pokémon ({{ filteredPokemon.length }})</h2>
        <ul class="space-y-2 max-h-96 overflow-y-auto">
          <li v-for="poke in filteredPokemon" :key="poke.name">
            <button
              @click="selectedPokemon = poke"
              :class="[
                'w-full text-left px-2 py-1 rounded text-sm',
                selectedPokemon?.name === poke.name
                  ? 'bg-indigo-500 text-white'
                  : 'hover:bg-gray-200',
              ]"
            >
              {{ capitalize(poke.name) }}
            </button>
          </li>
        </ul>
      </div>

      <!-- pokemon details -->
      <div class="lg:col-span-3">
        <div v-if="loadingDetails" class="p-3 text-gray-600">
          Chargement détails...
        </div>
        <div v-else-if="detailsError" class="p-3 text-red-600">
          {{ detailsError }}
        </div>
        <div
          v-else-if="selectedPokemonData"
          class="border rounded-lg p-6 bg-white shadow-sm"
        >
          <header class="flex items-start gap-6 mb-6">
            <img
              :src="selectedPokemonData.sprites.front_default"
              alt="sprite"
              class="w-32 h-32 object-contain"
            />
            <div class="flex-1">
              <h2 class="text-2xl font-semibold">
                {{ capitalize(selectedPokemonData.name) }}
                <small class="text-gray-500"
                  >#{{ selectedPokemonData.id }}</small
                >
              </h2>
              <p class="mt-2 flex flex-wrap gap-2">
                <span
                  v-for="t in selectedPokemonData.types"
                  :key="t.slot"
                  class="inline-block bg-indigo-100 text-indigo-700 px-3 py-1 rounded-full text-xs"
                >
                  {{ capitalize(t.type.name) }}
                </span>
              </p>
            </div>
          </header>

          <section class="mb-6">
            <h3 class="font-semibold mb-2">Abilities</h3>
            <ul class="list-disc list-inside space-y-1">
              <li
                v-for="a in selectedPokemonData.abilities"
                :key="a.ability.name"
                class="text-sm"
              >
                {{ capitalize(a.ability.name) }}
                <span v-if="a.is_hidden" class="text-xs text-gray-500"
                  >(hidden)</span
                >
              </li>
            </ul>
          </section>

          <section>
            <h3 class="font-semibold mb-3">Stats</h3>
            <ul class="grid grid-cols-2 gap-2">
              <li
                v-for="s in selectedPokemonData.stats"
                :key="s.stat.name"
                class="flex items-center justify-between bg-gray-50 p-3 rounded text-sm"
              >
                <span class="font-medium text-gray-700">{{
                  capitalize(s.stat.name)
                }}</span>
                <span class="font-bold text-indigo-600">{{ s.base_stat }}</span>
              </li>
            </ul>
          </section>
        </div>
        <div v-else class="text-center text-gray-400">
          Sélectionnez un pokémon
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed, watch } from "vue";

const loading = ref(true);
const error = ref("");
const pokemonList = ref([]);
const selectedPokemon = ref(null);
const selectedPokemonData = ref(null);
const loadingDetails = ref(false);
const detailsError = ref("");
const searchQuery = ref("");

function capitalize(s) {
  if (!s) return "";
  return s.charAt(0).toUpperCase() + s.slice(1).replace(/-/g, " ");
}

onMounted(async () => {
  try {
    const res = await fetch("https://pokeapi.co/api/v2/pokemon?limit=1025");
    if (!res.ok) throw new Error(`${res.status} ${res.statusText}`);
    const data = await res.json();
    pokemonList.value = data.results;
    if (pokemonList.value.length > 0) {
      selectedPokemon.value = pokemonList.value[0];
      await fetchPokemonDetails(selectedPokemon.value);
    }
  } catch (err) {
    error.value = err.message || "Impossible de récupérer la liste";
  } finally {
    loading.value = false;
  }
});

async function fetchPokemonDetails(poke) {
  loadingDetails.value = true;
  detailsError.value = "";
  selectedPokemonData.value = null;
  try {
    const res = await fetch(poke.url);
    if (!res.ok) throw new Error(`${res.status} ${res.statusText}`);
    selectedPokemonData.value = await res.json();
  } catch (err) {
    detailsError.value = err.message || "Impossible de récupérer les détails";
  } finally {
    loadingDetails.value = false;
  }
}

// watch selectedPokemon
watch(selectedPokemon, (newPoke) => {
  if (newPoke) {
    fetchPokemonDetails(newPoke);
  }
});

const filteredPokemon = computed(() => {
  if (!searchQuery.value) return pokemonList.value;
  const query = searchQuery.value.toLowerCase();
  return pokemonList.value.filter((poke) =>
    poke.name.toLowerCase().includes(query),
  );
});
</script>

<style scoped>
.page {
  max-width: 720px;
  margin: 32px auto;
  padding: 16px;
  font-family:
    system-ui,
    -apple-system,
    "Segoe UI",
    Roboto,
    "Helvetica Neue",
    Arial;
}
.msg {
  padding: 12px;
}
.error {
  color: #b00020;
}
.card {
  border: 1px solid #e5e7eb;
  border-radius: 8px;
  padding: 16px;
  background: #fff;
}
.card-header {
  display: flex;
  align-items: center;
  gap: 12px;
}
.sprite {
  width: 96px;
  height: 96px;
  image-rendering: pixelated;
}
.title h2 {
  margin: 0;
}
.types {
  margin: 6px 0 0;
}
.type {
  display: inline-block;
  background: #eef2ff;
  color: #3730a3;
  padding: 4px 8px;
  border-radius: 999px;
  font-size: 12px;
  margin-right: 6px;
}
.section {
  margin-top: 12px;
}
.stats {
  list-style: none;
  padding: 0;
  margin: 0;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
  gap: 8px;
}
</style>
