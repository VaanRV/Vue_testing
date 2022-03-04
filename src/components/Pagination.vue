<template>
  <div class="pagination">
    <div> {{ fromPage }} - {{ toPage }} de {{ totalData }} </div>
    <div class="pagination-buttons">
      <a class="pagination-previous" v-on:click="changePage( 1 )">
        <img class="first-page" alt="First page" src="../assets/first_page.png" />
      </a>
      <a class="pagination-previous" v-on:click="changePage( page - 1)">
        <img class="first-page" alt="First page" src="../assets/left_arrow.png" />
      </a>
      <a>{{ page }}</a>
      <a class="pagination-next" v-on:click="changePage( page + 1)">
        <img class="first-page" alt="First page" src="../assets/right_arrow.png" />
      </a>
      <a class="pagination-next" v-on:click="changePage( lastPage )">
        <img class="first-page" alt="First page" src="../assets/last_page.png" />
      </a>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Pagination',
    props: [ 'page', 'changePage', 'fromPage', 'toPage', 'totalData', 'lastPage' ]
  }
</script>

<style scoped>
  .pagination {
    display: flex;
    justify-content: flex-end;
    padding: 10px 0;
    box-shadow: inset 0 -2px 0 0 rgba(100,121,143,0.122);
    color: #5f6368;
    filter: invert(42%) sepia(11%) saturate(230%) hue-rotate(174deg) brightness(86%) contrast(91%);
  }
  .pagination-buttons {
    display: flex;
    justify-content: space-between;
    margin-left: 10px;
    width: 150px;
  }
  .first-page {
    background-position: center;
    background-repeat: no-repeat;
    background-size: 20px;
  }
</style>
