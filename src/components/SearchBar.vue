<template>
  <!-- Barra de busqueda  -->
  <div class="search-bar-section">
    <input type="text" class="search-bar" v-model="searchText" placeholder="Buscador" @keyup.enter="filterList()" />
    <img class="search-image" alt="First page" src="../assets/search.png" width="25" height="25" v-on:click="filterList()" />
    <!-- Tipo de campo para la busqueda -->
    <select class="select-option" v-model="typeSearch">
      <option disabled value="">Selecciona la busqueda</option>
      <option v-for="option in options" :value="option.value" :key="option.value">
        {{ option.text }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  name: 'SearchBar',
  props: [ 'apiData', 'portsData', 'totalResults', 'toPage', 'fromPage', 'totalData' ],
  updated() { //Asignamiento de los datos para ser manejados dentro del componente
    this.apiDataMutate = this.apiData
    this.portsDataMutate = this.portsData
    this.toPageMutate = this.toPage
    this.fromPageMutate = this.fromPage
    this.totalResultsMutate = this.totalResult
    this.totalDataMutate = this.totalData
  },
  data() {
      return {
          searchText: '',
          typeSearch: 'name',
          orderData: [],
          apiDataMutate : [],
          portsDataMutate: [],
          toPageMutate: null,
          fromPageMutate: null,
          totalResultsMutate: null,
          totalDataMutate: null,
          emptySearch: true,
          options: [ //Opciones para el tipo de busqueda
            { text: 'Id', value: 'id' },
            { text: 'Nombre', value: 'name' },
            { text: 'País', value: 'country' },
            { text: 'Continente', value: 'continent' },
            { text: 'Coodinadas', value: 'coordinates'}
          ]
      }
  },
  methods: {
    filterList() { //Filtrado de los datos de acuerdo al texto y el tipo de busqueda
      this.orderData = []
      if(this.searchText.length > 0) {
        if(this.typeSearch !== 'id' && this.typeSearch !== 'coordinates') {
          this.apiDataMutate.filter(port => {
            port[this.typeSearch].toLowerCase().includes(this.searchText.toLowerCase()) && this.orderData.push(port)
          })
        }
        else {
          this.apiDataMutate.filter(port => {
            if(port[this.typeSearch] !== null) {
              port[this.typeSearch].toString().includes(this.searchText) && this.orderData.push(port)
            }
          })
        }
        //Asignación de la data filtrada y la cantidad de resultados actuales
        this.portsDataMutate = this.orderData
        this.totalResultsMutate = this.toPageMutate = this.portsDataMutate.length
        this.fromPageMutate = this.portsDataMutate.length > 0 ? 1 : 0
        this.emptySearch = false
      }
      else {
        //Asignación de la data filtrada sin busqueda
        this.portsDataMutate = this.apiDataMutate
        this.emptySearch = true
      }
      //Enviar la nueva data filtrada por busqueda al componente TableData
      this.$emit('searchRecieve', this.portsDataMutate, this.toPageMutate, this.fromPageMutate, this.totalResultsMutate, this.emptySearch)
    },
  },
}
</script>

<style scoped>
  .select-option {
    position: absolute;
    right: 0;
    background: transparent;
    border: unset;
    border-left: 1px solid black;
    height: 75%;
    margin-right: 5px;
    padding-left: 10px;
  }
  select {
    cursor: pointer;
  }
  .search-section {
    display: flex;
    align-items: center;
  }
  .search-bar-section {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 720px;
    height: 45px;
    margin: 10px 0;
    background: #f1f3f4;
    border: 1px solid transparent;
    border-radius: 8px;
    position: relative;
  }
  .search-bar {
    width: 100%;
    background: transparent;
    border: none;
    padding-left: 40px;
    height: 45px;
    outline: none;
  }
  .search-bar:focus {
    background: white;
    box-shadow: 1px 1px 1px 1px rgba(134,141,155,0.2);
    border-radius: 8px;
  }
  .search-image {
    position: absolute;
    left: 0;
    padding-left: 10px;
    cursor: pointer;
  }
</style>
