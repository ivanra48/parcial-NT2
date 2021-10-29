<template>

  <section class="src-components-formulario">
    <h2>Formulario</h2>
    <hr>
    <vue-form :state="formState" @submit.prevent="enviar()">
      <validate tag="div">
        <label for="nombre" >Nombre</label>
        <input type="text" id="nombre" name="nombre" autocomplete="off" class="form-control" v-model.trim="formData.nombre" :minlength="nombreMinLength" :maxlength="nombreMaxLength" required no-espacios>
        <field-messages name="nombre" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
            <div slot="no-espacios" class="alert alert-danger mt-1">No se permiten espacios en el campo</div>
            <div slot="minlength" class="alert alert-danger mt-1">Este campo requiere al menos {{ nombreMinLength }} caracteres</div>
            <div v-if="formData.nombre.length == nombreMaxLength" class="alert alert-danger mt-1">Este campo debe tener como máximo {{ nombreMaxLength }} caracteres</div>
          </field-messages>
      </validate>

      <validate tag="div">
        <label for="descripcion" >Descripción</label>
        <input type="text" id="descripcion" name="descripcion" autocomplete="off" class="form-control" v-model.trim="formData.descripcion" required>
        <field-messages name="descripcion" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
          </field-messages>
      </validate>

      <validate tag="div">
        <label for="importe" >Importe</label>
        <input type="number" id="importe" name="importe" autocomplete="off" class="form-control" v-model.number="formData.importe" required>
        <field-messages name="importe" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>            
          </field-messages>
      </validate>
      <button class="btn btn-success mt-3 ml-3 mb-3" type="submit" :disabled="formState.$invalid">Enviar</button>
    </vue-form>

    <div>
      <label for="presupuesto" >Presupuesto</label>
      <input type="number" id="presupuesto" name="presupuesto" autocomplete="off" class="form-control" v-model.number="formData.presupuesto">
    </div>

      <div v-if="gastos.length" class="table-responsive">
        <table class="table table-bordered">
          <thead>
            <tr>
              <th v-for="(col,index) in getCols" :key="index">{{ col }}</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(gastos, index) in gastos" :key="index">
              <td v-for="(col,index) in getCols" :key="index" >{{gastos[col]}}</td>              
            </tr>
            <tr :style="{color: analizarSaldo(sumatoria, presupuesto).color }">
              <td>Total</td>
              <td>${{ sumatoria }}</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div v-else class="alert alert-danger">No se encontraron gastos</div>
    
  </section>

</template>

<script lang="js">

  export default  {
    name: 'src-components-formulario',
    props: [],
    mounted () {

    },
    data () {
      return {
        formData : this.getInitialData(),
        formState : {},
        nombreMinLength : 3,
        nombreMaxLength : 15,
        gastos : [],
        total : 0 ,
        presupuesto : 0,
      }
    },
    methods: {
      getInitialData() {
        return {
          nombre: '',
          descripcion: '',
          importe: '',
          fecha: '',
        }
      },  

      enviar(){
        let consumo = {
          nombre: this.formData.nombre,
          descripcion: this.formData.descripcion,
          importe : this.formData.importe,
          fecha : new Date().toLocaleString()
        }
        console.log(consumo)
        
        this.gastos.push(consumo)
        this.formData = this.getInitialData();
        this.formState._reset();
      },

      analizarSaldo(sum, presu) {
        let total = sum
        let color = '#080'
        if(total > 1000) color = '#E5097F'
        if(total > 5000) color = '#ffa500'
        if( presu > 0){
          if (total > presu){
            color = '#FF0000'
          }
        }         
        return {          
          color
        }
      }
      
    },
    computed: {
      getCols() {
        return Object.keys(this.gastos[0])
      },

     sumatoria : function () {
                    var sum = 0;
                    this.gastos.forEach(e => {
                        sum += e.importe;
                    });
                    return sum
                  },
    }
}


</script>

<style scoped lang="css">
  .src-components-formulario {

  }

  input {
    margin: 5px;
  }
</style>
