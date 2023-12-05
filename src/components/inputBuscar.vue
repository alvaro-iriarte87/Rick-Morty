<template>
  <div class="buscador-container">
    <input v-model="searchTerm" class="input-buscador" placeholder="Buscar por letra...">
    <button @click="searchData" class="boton-buscar">Buscar</button>
    <div id="characters" class="card-container">
      <div v-for="character in filteredCharacters" :key="character.id" class="card">
        <img class="card-image" :src="character.image" :alt="character.name">
        <div class="card-content">
          <h1>{{ character.name }}</h1>
          <p><b>Tipo de personaje:</b> {{ character.species }}</p>
          <p><b>Estado:</b> {{ character.status }}</p>
          <p><b>Genero:</b> {{ character.gender }}</p>
          <p><b>Origen:</b> {{ character.origin.name }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchTerm: '',
      characters: [],
    };
  },
  computed: {
  filteredCharacters() {
    const regex = new RegExp(this.searchTerm, 'i');
    return this.characters.filter(character => regex.test(character.name));
  }
},
  methods: {
    async searchData() {
      try {
        const response = await fetch('https://rickandmortyapi.com/api/character/?page');

        // console.log(response)

        if (response.ok) {
          const datos = await response.json();
          this.characters = datos.results;
           
          // console.log(this.characters)

        } else {
          console.log('Error al obtener datos de la API');
        }
      } catch (error) {
        console.log('Error en la búsqueda:', error);
      }
    },
  },
};
</script>

<style scoped>
.buscador-container {
  display: grid;
  grid-template-columns: 1fr auto;
  gap: 1px;
  align-items: center;
  justify-content: center;
  max-width: 800px;
  margin: 0 auto;
}

.input-buscador {
  flex-grow: 0.7;
  padding: 12px;
  border: 2px solid #ccc;
  border-radius: 10px;
  margin-right: 2px;
  transition: border-color 0.3s ease;
}

.input-buscador:hover {
  border-radius: 14px;
  border-color: blue;
}

.boton-buscar {
  padding: 8px 26px;
  background-color: blue;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 10px;
  transition: background-color 0.3s ease;
}

.boton-buscar:hover {
  background-color: darkblue;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}

.card {
  width: 30%;
  margin-bottom: 20px;
  border: 2px solid blue;
  border-radius: 10px;
  padding: 10px;
}

.card-image {
  width: 100%;
  border-radius: 10px;
  margin-bottom: 10px;
}

.card-content {
  text-align: center;
}
</style>
