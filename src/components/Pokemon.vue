<template>
  <div class="container">
    <div class="header">
      <PokemonSearch :apiUrl="pokemonUrlPure" @setPokemonUrl="setPokemonUrl" />
          <p class="title">Pokémons</p>

    </div>
    <PokemonList :apiUrl="apiUrl" :pokemonUrlPure="pokemonUrlPure" @setPokemonUrl="setPokemonUrl" />

    <PokemonDetail
      v-if="showDetail"
      :pokemonUrl="pokemonUrl"
      @closeDetail="closeDetail"
    />
  </div>
</template>
<script>
import PokemonSearch from "./PokemonSearch.vue";
import PokemonList from "./PokemonList.vue";
import PokemonDetail from "./PokemonDetail.vue";

export default {
  data: () => {
    return {
      apiUrl: "https://pokeapi.co/api/v2/pokemon?offset=0&limit=50",
      pokemonUrlPure: "https://pokeapi.co/api/v2/pokemon/",
      pokemonUrl: "",
      showDetail: false,
    };
  },
  components: {
    PokemonSearch,
    PokemonList,
    PokemonDetail,
  },
  methods: {
    setPokemonUrl(url) {
      this.pokemonUrl = url;
      this.showDetail = true;
    },
    closeDetail() {
      this.pokemonUrl = "";
      this.showDetail = false;
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  padding: 10px;
  font-family: "Mulish", arial;
}

h1 {
  color: #0127ff;
  padding: 20px;
}
.title {
  font-size:14px;
  font-weight:bold;
  margin:48px 0 36px 0;
}

.header {
  display: flex;
  flex-direction: column;
  justify-content: left;
  align-items: left;
  padding-bottom: 0px;
}
</style>