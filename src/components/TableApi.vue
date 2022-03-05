<template>
  <div class="table-component">
    <div class="search-section">
      <div class="search-bar-section">
        <input type="text" class="search-bar" v-model="searchText" placeholder="Buscador" @keyup.enter="filterList(typeSearch)" />
        <img class="search-image" alt="First page" src="../assets/search.png" width="25" height="25" v-on:click="filterList(typeSearch)" />
        <SelectOptions :typeSearch="typeSearch" @returnType="recibeType" />
      </div>
    </div>
    <Pagination :page="page" :changePage="changePage" :fromPage="fromPage" :toPage="toPage" :totalResults="totalResults" :lastPage="lastPage"  />
    <table>
      <tbody>
        <tr>
          <th>
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
        <tr v-for="port in portsData" :key="port.id">
          <td> {{ port.id }} </td>
          <td> {{ port.name }} </td>
          <td> {{ port.country }} </td>
          <td> {{ port.continent }} </td>
          <td> {{ port.coordinates }} </td>
        </tr>
      </tbody>
    </table>
    <Pagination :page="page" :changePage="changePage" :fromPage="fromPage" :toPage="toPage" :totalResults="totalResults" :lastPage="lastPage" />
  </div>
</template>

<script>
import axios from 'axios';
import Pagination from './Pagination.vue';
import SelectOptions from './SelectOptions.vue';

export default {
  components: { Pagination, SelectOptions },
  name: 'TableApi',
  data(){
      return {
          apiData: [],
          searchText: '',
          portsData: [],
          orderArray: [],
          typeSearch: 'name',
          page: 1,
          lastPage: 25,
          fromPage: 0,
          toPage: 0,
          totalData: null,
          totalResults: null,
          typeOrder: true,
      }
  },
  mounted() {
    this.getPortsData()
  },
  methods: {
    getPortsData() {
      const params = {
        page: this.page
      }

      axios
        .get('http://apitest.cargofive.com/api/ports?page=', { params })
          .then(response => {
            this.apiData = response['data'].data
            this.portsData = response['data'].data
            this.last_page = response['data'].meta['last_page']
            this.fromPage = response['data'].meta['from']
            this.toPage = response['data'].meta['to']
            this.totalData = response['data'].meta['total']
            this.totalResults = response['data'].meta['total']
          })
          .catch(error => {
            console.log(error)
          })
    },
    changePage(page) {
      this.page = (page <= 0 || page >this.last_page ? this.page : page )
      this.getPortsData()
    },
    OrderByAsc(orderName) {
      this.orderArray = this.portsData.sort(((prev, next) => prev[orderName] > next[orderName]))
      this.portsData = this.portsData.sort((prev, next) => this.orderArray[prev[orderName]] - this.orderArray[next[orderName]])
      this.typeOrder = false
    },
    OrderByDesc(orderName) {
      this.orderArray = this.portsData.sort(((prev, next) => prev[orderName] < next[orderName]))
      this.portsData = this.portsData.sort((prev, next) => this.orderArray[prev[orderName]] - this.orderArray[next[orderName]])
      this.typeOrder = true
    },
    filterList(searchTerm) {
      console.log(searchTerm)
      this.orderArray = []
      if(this.searchText.length > 0) {
        if(searchTerm !== 'id' && searchTerm !== 'coordinates') {
          this.apiData.filter(port => {
            port[searchTerm].toLowerCase().includes(this.searchText.toLowerCase()) && this.orderArray.push(port)
          })
        }
        else {
          this.apiData.filter(port => {
            if(port[searchTerm] !== null) {
              port[searchTerm].toString().includes(this.searchText) && this.orderArray.push(port)
            }
          })
        }
        this.portsData = this.orderArray
        this.totalResults = this.toPage = this.portsData.length
        this.fromPage = this.portsData.length > 0 ? 1 : 0
      }
      else {
        this.portsData = this.apiData
        this.totalResults = this.totalData
        this.toPage = this.portsData.length
        this.fromPage = 1
      }
    },
    recibeType(value){
      this.typeSearch = value
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
</style>
