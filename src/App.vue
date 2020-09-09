<template>
  <div class="contenido">
    <!-- Navbar -->
    <div class="nav">
      <img
        src="../src/assets/img/logoPoke.png"
        width="400"
        alt=""
        class="logoPoke"
      />
      <img
        src="../src/assets/img/pikachu.png"
        height="100"
        alt=""
        class="dibujo"
      />
    </div>

    <!-- Búsqueda -->
    <div class="busqueda">
      <input
        v-model="nombre"
        type="text"
        @keyup.enter="buscarPokemon"
        class="inputBusqueda"
      />

      <button @click="buscarPokemon" class="btnBusqueda">Buscar Pokemon</button>
      <img
        src="../src/assets/img/pokebola.png"
        height="40"
        alt=""
        class="pokebola"
      />
    </div>

    <!-- Resultado Búsqueda Pokémon -->
    <div class="card">
      <div class="contIzq">
        <h4 class="tituloMov">Movimientos</h4>
        <p class="textos" v-for="(movimiento, i) in movimientos" :key="i">
          {{ movimiento.move.name }}
        </p>
      </div>

      <div class="contCentro">
        <h2 class="pokeNombre">{{ nombrePokemon }}</h2>
        <img :src="imagen" width="150" alt="" class="pokeImg" />
      </div>

      <div class="contDcha">
        <h4 class="tituloMov">Habilidades</h4>
        <p class="textos" v-for="habilidad in habilidades">
          {{ habilidad.ability.name }}
        </p>
      </div>
    </div>

    <!-- Mensaje error -->
    <div v-show="alert" class="error">
      El pokemon no existe
    </div>

  </div>
</template>


<script>
export default {
  // Nombre del componente
  name: "app",
  data() {
    return {
      // String vacío para guardar nombre del pokemon
      nombre: "",
      // Objeto pokemon en cuyas propiedades guardamos los datos solicitados 
      pokemon: {
        // Nombre lo guardo en un string
        name: "",
        // Imagen lo guardo en un string
        sprites: { front_default: "" },
        // Movimientos lo guardo en un array porque puede contener más de un elemento
        moves: [],
        //Habilidades en un array porque puede contener más de un elemento
        abilities: [],
      },
    };
  },
  // Uso de created para llamar a la base de datos antes de que se renderice algo
  created() {
    this.buscarPokemon();
  },

  methods: {
    // Función asíncrona para buscar pokemones en la API
    async buscarPokemon() {
      try {
        // Petición a la API pokemon con método fetch
        await fetch(this.url)
        // Espera a la llamada a la API 
          .then(function(response) {
            // Retorna data solicitada en formato Json
            return response.json();
          })
          // Guardo data recibida en formato Json en variable pokemon
          .then((myJson) => {
            console.log(myJson);
            this.pokemon = myJson;
          });
      // Error en llamado a la API
      } catch (error) {
        console.log("errorcirijillo");
        //this.alert = true;
        this.nombre = "pikachu";
        this.buscarPokemon();
      }
    },
  },
  computed: {
    imagen() {
      return this.pokemon.sprites.front_default;
    },
    nombrePokemon() {
      return this.pokemon.name;
    },
    movimientos() {
      return this.pokemon.moves.slice(0, 5);
    },
    habilidades() {
      return this.pokemon.abilities.slice(0, 5);
    },
    url() {
      return this.nombre == ""
        ? `https://pokeapi.co/api/v2/pokemon/pikachu`
        : `https://pokeapi.co/api/v2/pokemon/${this.nombre.toLowerCase()}`;
    },
  },
};
</script>

<style>

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: "Arvo", "Grandstander", "Indie Flower", serif;
}

.nav {
  background-color: rgb(221, 0, 48);
  display: flex;
  justify-content: center;
}

.dibujo {
  margin-top: 15px;
}

.contenido {
  margin: auto;
}

.card {
  display: flex;
  justify-content: center;
  padding-top: 80px;
}

.contIzq {
  display: inline-block;
  margin: 34px;
}

.contCentro {
  display: inline-block;
  justify-content: center;
  margin: 30px;
  text-align: center;
}

.contDcha {
  display: inline-block;
  margin: 34px;
  text-align: right;
}

.pokeNombre {
  font-size: 34px;
  color: rgb(255, 208, 0);
  font-family: "Grandstander";
}

.tituloMov {
  font-size: 24px;
  margin-bottom: 20px;
  color: rgb(19, 42, 148);
}

.textos {
  color: rgb(75, 106, 245);
  font-family: "Grandstander";
  margin: 10px 0;
  font-size: 18px;
}

.busqueda {
  display: flex;
  justify-content: center;
  margin-top: 15px;
}

.inputBusqueda {
  border-radius: 5px;
}

.btnBusqueda {
  border-radius: 5px;
  padding: 0 10px;
  color: azure;
  background-color: rgb(248, 100, 100);
  cursor: pointer;
  border-style: none;
}

.error {
  text-align: center;
  color: rgb(209, 0, 0);
}

.footer {
  background-color: rgb(248, 100, 100);
  height: 10%v;
}
</style>
