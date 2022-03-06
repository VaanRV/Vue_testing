<template>
  <div class="pagination">
    <div> {{ fromPage }} - {{ toPage }} de {{ totalResults }} </div> <!-- Total de resultados en la página -->
    <div class="pagination-buttons">
      <a v-bind:class="page === 1 ? 'navigation-arrow disabled-page' : 'navigation-arrow'" v-on:click="changePage( 1 )"> <!-- Navegación rapida a la primera página  -->
        <img alt="First page" src="../assets/first_page.png" />
      </a>
      <a v-bind:class="page === 1 ? 'navigation-arrow disabled-page' : 'navigation-arrow'" v-on:click="changePage( page - 1)"> <!-- Navegación a la página anterior  -->
        <img alt="previous page" src="../assets/left_arrow.png" />
      </a>
      <a> {{ page }} </a>
      <a  v-bind:class="page === lastPage ? 'navigation-arrow disabled-page' : 'navigation-arrow'" v-on:click="changePage( page + 1)"> <!-- Navegación a la siguiente página  -->
        <img alt="Next page" src="../assets/right_arrow.png" />
      </a>
      <a v-bind:class="page === lastPage ? 'navigation-arrow disabled-page' : 'navigation-arrow'" v-on:click="changePage( lastPage )"> <!-- Navegación rapida a la ultima página  -->
        <img alt="Last page" src="../assets/last_page.png" />
      </a>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Pagination',
    props: [ 'page', 'changePage', 'fromPage', 'toPage', 'totalResults', 'lastPage' ]
  }
</script>

<style scoped>
  .pagination {
    display: flex;
    justify-content: flex-end;
    padding: 10px 0;
    box-shadow: inset 0 -2px 0 0 rgba(100,121,143,0.122);
    color: #474648;
  }
  .pagination-buttons {
    display: flex;
    justify-content: space-between;
    margin-left: 10px;
    width: 150px;
  }
  .disabled-page {
    filter: invert(45%) sepia(20%) saturate(16%) hue-rotate(319deg) brightness(96%) contrast(83%) !important;
    pointer-events: none;

  }
  .navigation-arrow {
    cursor: pointer;
    filter: invert(33%) sepia(2%) saturate(2954%) hue-rotate(314deg) brightness(86%) contrast(91%);
  }
</style>
