<template>
  <div class="navbar">
    <div class="image-container">
      <img alt="CargoFive Logo" src="https://cargofive.com/wp-content/uploads/2018/07/logo.svg" width="150" height="70">
    </div>
    <div class="info-navbar">
      <a href="https://www.linkedin.com/in/francisco-duran-20424a22b/" target="_blank">Sobre mi</a>
      <a href="https://cargofive.com/es/#beneficios" target="_blank">Beneficios</a>
      <a href="https://cargofive.com/es/blog/" target="_blank">Blog</a>
      <a href="https://app.cargofive.com/login" target="_blank">Ingresar</a>
      <a href="https://cargofive.com/es/prueba-gratuita/" target="_blank" class="button-navbar">Prueba Gratis</a>
    </div>
  </div>
</template>

<style scoped>
  a {
    font-weight: bold;
    text-decoration: none;
    text-transform: none;
    font-size: 17px;
    line-height: 23.8px;
    font-weight: 500;
    color: white;
    padding: 0 20px;
  }
  .navbar {
      background-color: #575757;
      box-shadow: 0px 5px 5px -4px rgba(0,0,0,.2);
      height: 75px;
      display: flex;
      align-items: center;
      justify-content: space-around;
  }
  .info-navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 37%;
  }
  .image-container {
    padding: 15px;
    filter: invert(100%) sepia(0%) saturate(0%) hue-rotate(93deg) brightness(103%) contrast(103%);
  }
  .button-navbar {
    background: #356eea;
    padding: 7px 20px;
    border-radius: 4px;
  }
</style>
