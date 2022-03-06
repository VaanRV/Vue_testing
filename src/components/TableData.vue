<template>
  <!-- Tabla de datos -->
  <div class="table-component">
    <div class="search-section"> <!-- Barra de busqueda para el filtrado de datos-->
      <SearchBar :apiData="apiData" :portsData="portsData" :totalResults="totalResults" :toPage="toPage" :fromPage="fromPage" :totalData="totalData" @searchRecieve="searchPort" />
    </div>
    <!-- Paginación de la tabla -->
    <Pagination :page="page" :changePage="changePage" :fromPage="fromPage" :toPage="toPage" :totalResults="totalResults" :lastPage="lastPage"  />
    <table>
      <tbody>
        <tr>
          <!-- Campos de la tabla -->
          <th>
            <!-- Ordenamiento de los datos -->
            <a class="table-title" v-on:click="typeOrder ? OrderByAsc('id') : OrderByDesc('id')">
              ID <img alt="First page" src="../assets/order_icon.png" />
            </a>
          </th>
          <th>
            <a class="table-title" v-on:click="typeOrder ? OrderByAsc('name') : OrderByDesc('name')">
              Nombre <img alt="First page" src="../assets/order_icon.png" />
            </a>
          </th>
          <th>
            <a class="table-title" v-on:click="typeOrder ? OrderByAsc('country') : OrderByDesc('country')">
              País <img alt="First page" src="../assets/order_icon.png" />
            </a>
          </th>
          <th>
            <a class="table-title" v-on:click="typeOrder ? OrderByAsc('continent') : OrderByDesc('continent')">
              Continente <img alt="First page" src="../assets/order_icon.png" />
            </a>
          </th>
          <th>
            <a class="table-title" v-on:click="typeOrder ? OrderByAsc('coordinates') : OrderByDesc('coordinates')">
              Coordinadas <img alt="First page" src="../assets/order_icon.png" />
            </a>
          </th>
        </tr>
        <!-- Registros de la información de los puertos -->
        <tr v-for="port in portsData" :key="port.id">
          <td> {{ port.id }} </td>
          <td> {{ port.name }} </td>
          <td> {{ port.country }} </td>
          <td> {{ port.continent }} </td>
          <td> {{ port.coordinates }} </td>
        </tr>
      </tbody>
    </table>
    <!-- Paginación de la tabla -->
    <Pagination :page="page" :changePage="changePage" :fromPage="fromPage" :toPage="toPage" :totalResults="totalResults" :lastPage="lastPage" />
  </div>
</template>

<script>
import axios from 'axios';
import Pagination from './Pagination.vue';
import SearchBar from './SearchBar.vue';

export default {
  components: { Pagination, SearchBar },
  name: 'TableData',
  data(){
      return {
          apiData: [], //Data de todos los puertos
          portsData: [], //Data filtrada u ordenada de los puertos
          orderData: [],  //Arreglo temporal data ordenada
          page: 1, //Página actual
          lastPage: 1, //Última página
          fromPage: 0, //
          toPage: 0,
          totalData: null, //Cantidad de datos entregada por la API
          totalResults: null, //Cantidad de datos filtrada
          typeOrder: true, //Estado para el tipo de ordenamiento
      }
  },
  created() {
    this.getPortsData() //Obtener los datos luego de que haya sido creado el componente
  },
  methods: {
    async getPortsData() { //Obtención de los datos desde la API de puertos
      const params = {
        page: this.page
      }
      axios
        .get('http://apitest.cargofive.com/api/ports?page=', { params })
          .then(response => {
            this.apiData = response['data'].data
            this.portsData = response['data'].data
            this.lastPage = response['data'].meta['last_page']
            this.fromPage = response['data'].meta['from']
            this.toPage = response['data'].meta['to']
            this.totalData = response['data'].meta
            this.totalResults = response['data'].meta['total']
          })
          .catch(error => {
            console.log(error)
          })
    },
    changePage(page) { //Cambio de página y obtención de los nuevos datos
      this.page = (page <= 0 || page > this.last_page ? this.page : page )
      this.getPortsData()
    },
    OrderByAsc(orderName) { //Ordenamiento ascedente de acuerdo al tipo de dato
      this.orderData = this.portsData.sort(((prev, next) => prev[orderName] > next[orderName]))
      this.portsData = this.portsData.sort((prev, next) => this.orderData[prev[orderName]] - this.orderData[next[orderName]])
      this.typeOrder = false
    },
    OrderByDesc(orderName) { //Ordenamiento descendente de acuerdo al tipo de dato
      this.orderData = this.portsData.sort(((prev, next) => prev[orderName] < next[orderName]))
      this.portsData = this.portsData.sort((prev, next) => this.orderData[prev[orderName]] - this.orderData[next[orderName]])
      this.typeOrder = true
    },
    searchPort(portsData, toPage, fromPage, totalResults, emptySearch) { //Asignar la nueva data de los puertos filtrados por busqueda
      if(emptySearch) {
        this.lastPage = this.totalData.lastPage
        this.totalResults = this.totalData.total
        this.toPage = this.totalData.to
        this.fromPage = this.totalData.from
        this.page = this.totalData.current_page
        this.lastPage = this.totalData.last_page
      }
      else {
        this.totalResults = totalResults
        this.toPage = toPage
        this.fromPage = fromPage
        this.page = 1
        this.lastPage = 1
      }
      this.portsData = portsData
    }
  },
}
</script>

<style scoped>
  table {
    border-collapse: collapse;
    width: 100%;
  }
  tr {
    height: 40px;
    box-shadow: inset 0 -2px 0 0 rgba(100,121,143,0.122);
  }
  td, th{
    width: 20%;
  }
  .table-component {
    padding: 0 15px;
  }
  .table-title {
    display: flex;
    justify-content: center;
    cursor: pointer;
  }
</style>
