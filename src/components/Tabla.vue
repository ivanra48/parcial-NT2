<template>

  <section class="src-components-tabla">
    <table class="table table-bordered">
      <thead>
        <tr>
          <th v-for="(col,index) in getCols" :key="index">{{ col }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(respuesta, index) in respuestas" :key="index">
          <td v-for="(col,index) in getCols" :key="index">{{respuesta[col]}}</td>
        </tr>
      </tbody>
    </table>
  </section>

</template>

<script lang="js">

  export default  {
    name: 'src-components-tabla',
    props: [
    ],
    mounted () {

    },
    data () {
      return {
        respuestas : [
          {  numero: 1, respuestas: 'D'},
          {  numero: 2, respuestas: 'C,D'},
          {  numero: 3, respuestas: 'B'}
        ]
      }
    },
    methods: {

      
    },
    computed: {
      getCols() {
        return Object.keys(this.respuestas[0])
      }
    }
}


</script>

<style scoped lang="css">
  .src-components-tabla {

  }
</style>
