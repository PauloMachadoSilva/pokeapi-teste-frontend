<template>
  <div class="searchbar">
    <form @submit.prevent="setPokemonUrl">
      <input
        type="text"
        class=""
        placeholder="Pesquise por nome ou código"
        v-model="searchvalue"
      />
    </form>
    <i class="fas fa-search" @click="setPokemonUrl"></i>
  </div>
</template>

<script>
export default {
  props: ["apiUrl"],
  data: () => {
    return {
      searchvalue: "",
    };
  },
  methods: {
    setPokemonUrl(url) {
      if (this.searchvalue !== "")
        this.$emit("setPokemonUrl", this.apiUrl + this.searchvalue);
    },
  },
};
</script>

<style lang="scss" scoped>
.searchbar {
  position: relative;
  width: 100%;
  padding-bottom: 20px;
  margin-top: 20px;
}
input {
  border: none;
  outline: none;
  border-radius: 50px;
  padding: 10px 40px 10px 36px;
  width: calc(100% - 50px);
  font-size: 1rem;
  border: 1px solid #ccc;
}
.legenda {
  color: #fff;
}
i {
  position: absolute;
  top: 10px;
  right: 10px;
  font-size: 1.25rem;
  color: #0a2e50;
  cursor: pointer;
}
</style>
