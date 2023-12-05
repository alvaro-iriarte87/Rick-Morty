<template>
  <div>
    <header>
      <div class="titulo-header">
        <h1> Rick & Morty <b>WIKI</b></h1>
      </div>
      <div class="botones-header">
        <button class="button" @click="mostrarContenido(0)">Characters</button>
        <button class="button" @click='mostrarEpisodios(1)'>Episode</button>
        <button class="button" @click="mostrarUbicacion(2)">Location</button>
      </div>
    </header>
    <div v-if="contenidoVisible !== null">
      <h2 style="justify-content: center;text-align: center;">{{ contenidos[contenidoVisible] }}</h2>
    </div>

    <div class="componente">
      <InputBuscador />
    </div>
    <main>
      <div id="characters" class="contenedor">
      </div>
    </main>
  </div>
</template>

<script >
import InputBuscador from './components/inputBuscar.vue'

export default {
  components: {
    InputBuscador
  },
  data() {
    return {
      contenidoVisible: null,
      contenidos: [
        "Characters",
        "Episode",
        "Location"
      ],
    };
  },
  methods: {
    async mostrarContenido(indice) {
      this.contenidoVisible = indice;
      try {
        const respuesta = await fetch('https://rickandmortyapi.com/api/character');

        // console.log(respuesta)
        if (respuesta.status === 200) {
          const datos = await respuesta.json();
          // console.log(datos)
          let personajes = '';
          datos.results.forEach(personaje => {
            personajes += `
              <div class="card">
                <h1>${personaje.name}</h1>
                <img class="imagen-card" src="${personaje.image}" alt="${personaje.name}">
              </div>
            `;
          });
          document.getElementById('characters').innerHTML = personajes;
        } else if (respuesta.status === 404) {
          console.log('Error al cargar la API');
        } else {
          console.log('Ocurrió un error desconocido');
        }
      } catch (error) {
        console.log(error);
      }
    },

    async mostrarEpisodios(indice) {
      this.contenidoVisible = indice;
      try {
        const respuestaEpisodios = await fetch('https://rickandmortyapi.com/api/episode');
        const respuestaPersonajes = await fetch('https://rickandmortyapi.com/api/character');

        if (respuestaEpisodios.status === 200 && respuestaPersonajes.status === 200) {
          const datosEpisodios = await respuestaEpisodios.json();
          const datosPersonajes = await respuestaPersonajes.json();

          // console.log(datosEpisodios);
          // console.log(datosPersonajes);

          const episodiosFiltrados = datosEpisodios.results.map(episode => ({
            id: episode.id,
            name: episode.name,
          }));

          // console.log(episodiosFiltrados)

          const personajesFiltrados = datosPersonajes.results.map(character => ({
            id: character.id,
            image: character.image,
          }));

          // console.log(personajesFiltrados)

          let Episodes = '';
          episodiosFiltrados.forEach(episode => {
            const personajeCoincide = personajesFiltrados.find(character => character.id === episode.id);

            console.log(personajeCoincide)

            if (personajeCoincide) {
              Episodes += `
            <div class="card">
              <h1>${episode.name}</h1>
              <img class="imagen-card" src="${personajeCoincide.image}" alt="${episode.name}">
            </div>`;
            }
          });

          document.getElementById('characters').innerHTML = Episodes;

        } else if (respuestaEpisodios.status === 404 || respuestaPersonajes.status === 404) {
          console.log('Error al cargar la API');
        }

      } catch (error) {
        console.log(error);
      }
    },

    async mostrarUbicacion(indice) {
      this.contenidoVisible = indice;
      try {
        const respuestaUbicaciones = await fetch('https://rickandmortyapi.com/api/location');
        const respuestaPersonajes = await fetch('https://rickandmortyapi.com/api/character');

        if (respuestaUbicaciones.status === 200 && respuestaPersonajes.status === 200) {
          const datosUbicaciones = await respuestaUbicaciones.json();
          const datosPersonajes = await respuestaPersonajes.json();

          // console.log(datosUbicaciones)
          // console.log(datosPersonajes)

          const ubicacionesFiltradas = datosUbicaciones.results.map(location => ({
            id: location.id,
            name: location.name,
          }));

          // console.log(ubicacionesFiltradas)

          const personajesFiltrados = datosPersonajes.results.map(character => ({
            id: character.id,
            image: character.image,
          }));

          // console.log(personajesFiltrados)

          let Locations = '';
          ubicacionesFiltradas.forEach(location => {
            const personajeCoincide = personajesFiltrados.find(character => character.id === location.id);

            // console.log(personajeCoincide)
            if (personajeCoincide) {
              Locations += `
            <div class="card">
              <h1>${location.name}</h1>
              <img class="imagen-card" src="${personajeCoincide.image}" alt="${location.name}">
            </div>`;
            }
          });

          // console.log(personajeCoincide)

          document.getElementById('characters').innerHTML = Locations;

        } else if (respuestaUbicaciones.status === 404 || respuestaPersonajes.status === 404) {
          console.log('Error al cargar la API');
        }

      } catch (error) {
        console.log(error);
      }
    }

  }
};

</script>

<style>
#app {
  margin-top: 20px;
}

.poster {
  width: 80%;

}

body {
  margin: 0;
  padding: 0;
  font-family: Arial, sans-serif;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0;
  background-color: #f0f0f0;
}

h1 {
  margin: 0;
  font: oblique bold 170% cursive;
}

b {
  color: #528cc5;
  font: oblique bold 100% cursive;
}

.botones-header {
  display: flex;

}

.botones-header button {
  margin-left: 15px;
  border: none;
  background-color: transparent;
  padding: 10px;
  cursor: pointer;
  padding: 8px;
  position: relative;
  transition: color 0.3s;
  font: 140% sans-serif;
}

.button:hover {

  background-color: #ddd;
  color: blue;
}

.button:hover::after {
  content: '';
  display: block;
  width: 100%;
  height: 2px;
  background-color: blue;
  position: absolute;
  bottom: 0;
  left: 0;
}

.buscador-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.input-buscador:hover {
  border-radius: 14px;
  border-color: blue;
}

.boton-buscar {
  padding: 8px 16px;
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

.card {
  display: inline-table;
  margin-bottom: 20px;
  border-radius: 15px;
}

.card-image{
  width: auto;
}
</style>
