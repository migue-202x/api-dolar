<template>
<!-- 
  PARA CONSUMIR APIS ES NECESARIO TENER INSTALADO "AXIOS", por eso se debe importar= 
  <script>
    import axios from 'axios';

  Todo lo que se valla a trabajar con Vuetify tiene que estar encerrado en la etiqueta v-app 
  v-layout =  row
  v-flex = col
  
  Puntos de quiebre (BreakPoints): xs, sm, md, lg, xl
  -->
  <div>
    <v-layout :wrap="true">
      <v-flex xs12>
        <v-card>
          <v-date-picker 
            v-model="fecha" 
            full-width
            locale="es-ar"
            :min="minimo"
            :max="maximo"
            color="green lighten-1"
            @change="[getFecha(fecha), getDolar()]"
          ></v-date-picker>
        </v-card>
        <!-- valor del dolar -->
        <v-card color="#385F73" dark>
          <v-card-text class="display-1 text-center">
            {{ dolar }}  | {{ ddmmyy }}
          </v-card-text>
        </v-card>
        <!-- fin valor dolar -->
      </v-flex>
    </v-layout>
  </div>
</template>

<script>
  import axios from 'axios';
  import {mapMutations} from 'vuex';
  
  export default {
    data(){
      return{
        //fecha es asi para que cada vez que reiniciamos la pagina obtenga la fecha actual
        fecha:new Date().toISOString().substr(0, 10),
        ddmmyy:'',
        minimo: '1984',
        maximo: new Date().toISOString().substr(0, 10),
        dolar: null
      }
    },
    methods: {
      ...mapMutations(['mostrarLoading', 'ocultarLoading']),
      
      async getDolar(){
        try {
          //ante de consumir la api
          this.mostrarLoading({ titulo: 'Cargando...', color: 'secondary' })
          //await esperor que finalice -> xios.get('https://mindicador.cl/api/dolar') para continuar cun nuestro codigo
          let dolar = await axios.get(`https://mindicador.cl/api/dolar/${this.ddmmyy}`)
          if(dolar.data.serie.length != 0){
            this.dolar = '$' + await dolar.data.serie[0].valor
          }else{
            this.dolar = 'Sin resultados'
          }
        } catch (error) {
          //console.log(error)
        }
        finally{
          this.ocultarLoading()
        }
      },
      getFecha(fecha){
        //let arrayFecha = dia.split(['-'])
        //this.ddmmyy = arrayFecha[2] + '-' + arrayFecha[1] + '-' + arrayFecha[0] 
        this.ddmmyy = fecha.split('-').reverse().join('-')
      }
    },
    //cuando se nos cree nuestro documento
    created() {
      this.getFecha(this.fecha)
      this.getDolar()
    }
  }
</script>













