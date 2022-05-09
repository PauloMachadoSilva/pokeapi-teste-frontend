<template>
  <div class="list">
    <article
      v-for="(pokemon, name) in pokemons"
      :key="'poke' + name"
      @click="setPokemonUrl(pokemon.id)"
    >
    <div class="content">
      <img :src="pokemon.sprite" width="96" height="96" alt="" />
      <p class="name">{{ pokemon.name }}</p>
      <span>Cód: {{ pokemon.id }} </span>
      <div class="content-type">
        {{ pokemon.type }}
      </div>
    </div>
    </article>
    <div id="scroll-trigger" ref="infinitescrolltrigger">
      <i class="fas fa-spinner fa-spin"></i>
    </div>
  </div>
</template>

<script>
export default {
  props: ["apiUrl","pokemonUrlPure"],
  data: () => {
    return {
      pokemons: [],
      next: "",
      currentUrl: "",
    };
  },
  methods: {
    fetchData() {
      let req = new Request(this.currentUrl);
      fetch(req)
        .then((resp) => {
          if (resp.status === 200) return resp.json();
        })
        .then((data) => {
          this.nextUrl = data.next;
          data.results.forEach((pokemon) => {
              fetch(pokemon.url)
                .then((resp) => {
                  if (resp.status === 200) return resp.json();
                })
                .then((data) => {
                    data.types.forEach((type)=>{
                      pokemon.type =  type.type.name + (pokemon.type != undefined ? " / "+ pokemon.type: ""); 
                    })
                    pokemon.sprite = data.sprites.front_default;
                    pokemon.id = data.id;
                    this.pokemons.push(pokemon);
                })
                .catch((error) => {
                  console.log(error);
                });
          });
        });
    },
    scrollTrigger() {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (entry.intersectionRatio > 0 && this.nextUrl) {
            this.nextList();
          }
        });
      });

      observer.observe(this.$refs.infinitescrolltrigger);
    },
    nextList() {
      this.currentUrl = this.nextUrl;
      this.fetchData();
    },
    setPokemonUrl(id) {
      this.$emit("setPokemonUrl", this.pokemonUrlPure + id);
    },
  },
  created() {
    this.currentUrl = this.apiUrl;
    this.fetchData();
  },
  mounted() {
    this.scrollTrigger();
  },
};
</script>

<style lang="scss" scoped>
.list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(153px, 1fr));
  grid-gap: 10px;
  width: 100%;
}
article {
  height: 230px;
  background-color: #ffffff;
  text-align: center;
  text-transform: capitalize;
  border-radius: 8px;
  cursor: pointer;
  box-shadow: 0 4px 20px #E1E1E1;
}
h3 {
  margin: 0;
}
.name {
  font-size: 16px;
  font-weight:700;
}
.content {
  display: inline-grid;
  position:relative;
  width:100%;
  span {
    font-weight:bold;
    font-size:9px;
  }
  .content-type {
    margin: 50px auto;
    font-size:10px;
    color: #C4C4C4;
    width:90%;
    border-radius:5px;
    background: #F7F7F766;
  }
  
}
img {
  margin: 0 auto;
}
#scroll-trigger {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 150px;
  font-size: 2rem;
  color: #efefef;
}
</style>

