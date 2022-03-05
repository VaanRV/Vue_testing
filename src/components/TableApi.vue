<template>
  <div class="table-component">
    <div class="search-section">
      <SelectOptions :apiData="apiData" :portsData="portsData" :totalResults="totalResults" :toPage="toPage" :fromPage="fromPage" :totalData="totalData" @searchRecieve="searchPort" />
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
          portsData: [],
          orderArray: [],
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
    searchPort(portsData, toPage, fromPage, totalResults){
      this.portsData = portsData
      this.totalResults = totalResults
      this.toPage = toPage
      this.fromPage = fromPage
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
</style>
