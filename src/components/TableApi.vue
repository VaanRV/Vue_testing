<template>
  <div class="table-component">
      <Pagination :page="page" :changePage="changePage" :fromPage="fromPage" :toPage="toPage" :totalData="totalData" :lastPage="lastPage"  />
      <table>
          <tbody>
              <tr>
                <th>ID <a v-on:click="OrderBy('id')">Icono</a></th>
                <th>Nombre <a v-on:click="OrderBy('name')">Icono</a></th>
                <th>País <a v-on:click="OrderBy('country')">Icono</a></th>
                <th>Continente <a v-on:click="OrderBy('continent')">Icono</a></th>
                <th>Coordinadas <a v-on:click="OrderBy('coordinates')">Icono</a></th>
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
      <Pagination :page="page" :changePage="changePage" :fromPage="fromPage" :toPage="toPage" :totalData="totalData" :lastPage="lastPage" />
  </div>
</template>

<script>
import axios from 'axios';
import Pagination from './Pagination.vue';

export default {
  components: { Pagination },
  name: 'TableApi',
  data(){
      return {
          portsData: null,
          ordenamiento: null,
          arreglin: null,
          page: 1,
          lastPage: 25,
          fromPage: 1,
          toPage: 1,
          totalData:null
      }
  },
  mounted() {
    this.getPortsData()
    console.log(this.ordenamiento)
  },
  methods: {
    getPortsData() {
      const params = {
        page: this.page
      }

      axios
        .get('http://apitest.cargofive.com/api/ports?page=', { params })
          .then(response => {
            this.portsData = response['data'].data
            this.last_page = response['data'].meta['last_page']
            this.fromPage = response['data'].meta['from']
            this.toPage = response['data'].meta['to']
            this.totalData = response['data'].meta['total']
          })
          .catch(error => {
            console.log(error)
          })
    },
    changePage(page) {
      this.page = (page <= 0 || page >this.last_page ? this.page : page )
      this.getPortsData()
    },
    OrderBy(orderName) {
      this.ordenamiento = this.portsData.sort(((a, b) => a[orderName] > b[orderName]))
      console.log(this.ordenamiento)
      this.portsData = this.portsData.sort((a, b) => this.ordenamiento[a[orderName]] - this.ordenamiento[b[orderName]])
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
</style>
