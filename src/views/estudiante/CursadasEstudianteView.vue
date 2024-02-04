<template>
  <!-- <div class="container-fluid text-center"> -->
  <div class="container-fluid" id="contenido-global">
  <div class="row">
    <div class="mb-3 fw-bold">       

            <div class="mb-3 fs-4 text-center">                 
             HISTORIAL ACADEMICO DE AVANCE GENERAL
            </div>

            <div class=" mb-3">                 
              APELLIDOS Y NOMBRES:    {{`${apellidoP} ${apellidoM} ${nombres}`}}
            </div>
            
            <div class=" mb-3">                 
              CEDULA DE IDENTIDAD:     {{`${ci_estudiante}`}}
            </div>

            <div class=" mb-3">                 
              NRO DE REGISTRO:       {{`${numero_registro}`}}
            </div>

            <div class=" mb-3">                 
              CARRERA:         {{`${nombre_carrera}`}}
            </div>

            <div class=" mb-3">                 
              APELLIDOS Y NOMBRES:      {{`${apellidoP} ${apellidoM} ${nombres}`}}
            </div>

            <div class=" mb-3">                 
              FECHA DE EMISION :         {{`${fecha_emision}`}}
            </div>
            <div>
            <button   class="btn btn-warning" @click="clickMe">                           
              GENERAR PDF : <i class="fa-solid fa-file-pdf"></i>
              </button>
            </div>

     </div>
  </div>


   <div class="row">
      <!-- <div class="col-lg-12 col-sm-12 offset-lg-2 align-center"> -->
        <!-- <div class="col-lg-12 col-sm-12 align-center"> -->
          <div class="table-responsive text-center">
              <table  class="table table-bordered table-hover col-12">
                  <thead v-if="materias" class="pb-4">
                    <tr>
                      <th>
                        #
                      </th>
                      <th>
                        GESTION
                      </th>
                      <th>
                        SIGLA CODIGO
                      </th> 
                   
                      <th>
                        ASIGNATURA
                      </th>
                      <th >
                        HRS. ACAD.
                      </th>
                      
                      <th>
                        PRE-REQUISITOS
                      </th> 
                      <th>
                        CALIFICACION NUMERAL
                      </th>                 
                      <th>
                        ESTADO
                      </th>   
                      <!-- <th>
                        INS. DE APR.
                      </th>         
                      <th>
                        OBSERVACION
                      </th>                                            
                      <th>
                        ACCIONES
                      </th> -->
                    </tr></thead>
                  <tbody class="table-group-divider" id="contenido">
                    <tr v-for="materia,i  in materias" :key="materia">
                        <td>{{ i+1 }}</td>
                        <td>{{ materia.anio_cursado }}</td>
                        <td>{{ materia.codigo_asignatura }}</td>
                        <td>{{ materia.nombre_asignatura }}</td>
                        <td>{{ materia.total_horas }}</td>
                        <td>{{ materia.pre_requisitos }}</td>
                        <td>{{ materia.nota_num_final }}</td>
                        <td>{{ materia.estado_gestion_espaniol}}</td>
                        <!-- <td></td>
                        <td></td> -->
                        <!-- <td>{{ estudiante.nombre_asignatura }}</td> -->
                        <!-- <td >{{ estudiante.id_docente }}</td> -->

                        <!-- <td >{{ estudiante.estado_gestion_espaniol }}</td> -->
                          <!--  <td>{{ getCarrera(estudiante.id_carrera) }}</td> -->
                        <!-- <td>{{ estudiante.nota_num_final }}</td> -->

                        
                        <!-- <td>                                                                                 
                            <button   class="btn btn-warning" @click="clickMe"> -->                              
                              <!-- <i class="fa-solid fa-file-pdf"></i> -->
                            <!-- </button>                         
                        </td> -->
                    </tr>                    
                  </tbody>
              </table>
          <!-- </div> -->
      </div>
    </div>
  </div>

</template>

<script>
// @ is an alias to /src
//import {ref} from 'vue';
import axios from "axios";
import {show_alerta,confirmar1} from '../../funciones';
import { useRoute } from "vue-router";
import html2pdf from "html2pdf.js";


//import {ref} from 'vue';
// const provincias = computed(()=>{
//         return this.provincias = this.getProvincias()
// })
//const provincias = ref([]);
 //const contador =ref(0);
export default {
  name: 'AprobadasEstudianteView',
  data(){
    return {estudiantes:null,carreras:[],principal:'',
    ci_estudiante:'',nombres:'',apellidoP:'',apellidoM:'',numero_registro:'',nombre_carrera:'',fecha_emision:'',
    materias:null,message:'',
    url:'http://127.0.0.1:8000/estudiantes/obtenerAsignaturasCursadas'
  }
  },
  mounted(){
    const route =useRoute();
     this.id = route.params.id;
      
     this.url = this.url + '/' +this.id+'/';
    this.getMateriasCursadas();
    //ruta de navegacion despues de la accion eliminar
    this.principal='/estudiantes';
  },
  methods:{
    clickMe(){
      html2pdf(document.getElementById('contenido-global'),{
        margin:1,
        filename:'reporte.pdf',
        html2canvas:{scale:3}
      })
    },
    sortGestion(data){
        data = data.sort((a, b) => {
        if (a.anio_cursado < b.anio_cursado) {
          return -1;
        }
      });
      console.log(data);
      return data;
    },
    getMateriasCursadas(){
          axios.get(this.url)
            .then(            
                response =>{
                    
                    //if(!response.data.message){
                      //console.log('psando normal');
                    this.message = response.data.message,
                    this.ci_estudiante=response.data['estudiante']['ci_estudiante'],
                    this.nombres=response.data['estudiante']['nombres'],
                    this.apellidoP=response.data['estudiante']['apellidoP'],
                    this.apellidoM=response.data['estudiante']['apellidoM'],
                    this.numero_registro=response.data['estudiante']['numero_registro'],
                    this.nombre_carrera=response.data['estudiante']['nombre_carrera'],

                    this.fecha_emision=response.data['fecha_emision'],

                    this.materias = this.sortGestion(response.data['datos'])
                    
                    //this.materias = response.data['datos']
                    // }else{
                      // console.log('aki esta el error'+response.data.message);
                    // }

                  
                    
                }
            ).catch(error => { 
          console.log(error)
          show_alerta(this.message,'error')
          this.$router.push('/estudiantes')
        });
        console.log(this.materias+'sss');        
            
    },eliminar(id,nombre){
    //   for (let index = 0; index < 10; index++) {
    //     sendRequest('POST',{
    //     "id":999,
    //     "nombre_provincia": "TEST-PROVINCIAS",
    //     "id_departamento": null
    // },'http://127.0.0.1:8000/parametros/provincias/','ProvinciaS Eliminada');              
    //   }
      const ruta = 'estudiantes/estudiantes/'+id+'/';
      confirmar1(id,nombre,ruta,this.principal);      
    },
    getCarrera(id){      
      axios.get('http://127.0.0.1:8000/parametros/carreras/'+id+'/')
            .then(            
                response =>(
                    this.carreras[id] = response.data['nombre_carrera']
                )
            );
          return this.carreras[id]
    }
  }
}
</script>