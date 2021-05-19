<template>

  <!--todo lo que se valla a trabajar con Vuetify tiene que estar encerrado en la etiqueta v-app 
  v-layout =  Row
  v-flex = col

  Puntos de quiebre (BreakPoints): xs, sm, md, lg, xl
  -->
  <v-app>
    <v-content>
      <v-container>
        <v-dialog v-model="loading.estado" hide-overlay persistent width="300">
          <v-card color="primary" dark>
            <v-card-text>
              {{ loading.titulo }}
              <v-progress-linear indeterminate color="white" class="mb-0"></v-progress-linear>
            </v-card-text>
          </v-card>
        </v-dialog>
        
        <router-view/>
      </v-container>  
    </v-content>
  </v-app>
</template>

<script>
import {mapState} from 'vuex';

export default {
  name: 'App',
  data(){
    return{
      //
    }
  },
  computed: {
    ...mapState(['loading'])
  }
};  
</script>
