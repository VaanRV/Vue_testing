<template>
  <select class="select-option" v-model="typeSearch">
    <option disabled value="">Selecciona la busqueda</option>
    <option v-for="option in options" :value="option.value" :key="option.value" @click="sendType(option.value)">
      {{ option.text }}
    </option>
  </select>
</template>

<script>
export default {
  name: 'SelectOptions',
  props: [ 'typeSearch' ],
  data(){
      return {
          options: [
            { text: 'Id', value: 'id' },
            { text: 'Nombre', value: 'name' },
            { text: 'País', value: 'country' },
            { text: 'Continente', value: 'continent' },
            { text: 'Coodinadas', value: 'coordinates'}
          ]
      }
  },
  methods: {
    sendType(value) {
      this.$emit('returnType', value)
    }
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
</style>
