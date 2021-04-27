<template>
  <div id="app">
    <h1 class="fw-bold">Random Gif Cat</h1>
    <h2 class="mb-4">Arma tu gato</h2>
    <div class="container">
      <div class="row">
        <div class="col-12 col-md-4">
            <label class="form-label mt-3">Titulo: </label>
            <input class="form-control text-center" type="text" v-model="titulo">

            <label class="mt-3">Filtro: </label>
            <select class="form-select" type="text" v-model="filtro"> 
              <option v-for="f in filtros" :key="f">{{f}}</option>
            </select>

            <label class="mt-3">Color del texto: </label>
            <select class="form-select" type="text" v-model="color">
              <option value="red">Rojo</option>
              <option value="green">Verde</option>
              <option value="yellow">Amarillo</option>
              <option value="blue">Azul</option>
              <option value="white">Blanco</option>
            </select>

            <label class="mt-3" for="">Tamaño de la fuente: </label>
            <input class="form-control text-center" type="number" min="0" max="600" v-model="tamano">
            
            <button class="btn btn-dark my-4" @click="getCat">Obten tu Gato</button>
        </div>

        <div class="col-12 col-md-8">
          <h3 v-show="miss">Recuerda que debes llenar todos los campos</h3>
          <div v-show="texto">Un poco de paciencia, ya sabes, pocas veces un gato obedece a un humano</div>
          <div class="mx-auto" v-if="!image">
            <img src="../src/assets/arrow.gif" style="width: 300px" alt="">
            <h2 class="text-center">Tu gato va aqui</h2>
          </div>
          <div v-if="image">
            <img :src="image" alt="">
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios"

export default {
  name: "App",
  data() {
    return {
      filtros: ['blur', 'mono', 'sepia', 'negative', 'paint', 'pixel'],
      titulo: "wena profe",
      filtro: "sepia",
      color: "white",
      tamano: "500",
      image: "",
      texto: false,
      miss: false
    }
  },
  methods: {
    async getCat(){
      this.texto = true
      if (this.titulo == "" || this.filtro == "" || this.color == "" || this.tamano == "") {
        this.miss = true
      }  
      const url = `https://cataas.com/cat/gif/says/${this.titulo}?filter=${this.filtro}&color=${this.color}&size=${this.tamano}&type=or`
    
      try {
        const request = await axios(url)
        if (!request) return
        this.image = url
        this.texto = false
        this.miss = false

        console.log(this.image);     
      } catch (error) {
        console.log(`Error en la llamada a la API: ${error}`);
      }
    }
  },
  //created(){
  //  this.getCat()
  //}
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
