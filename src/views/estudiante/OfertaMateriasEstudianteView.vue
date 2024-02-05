<template>
  <!-- <div class="container-fluid text-center"> -->
  <div class="container-fluid" id="contenido-global" v-if="ofertaMaterias.length>0">
  <div class="row">
    <div class="mb-3 fw-bold">       

            <div class="mb-3 fs-4 text-center">                 
             OFERTA DE MATERIAS PARA LA INSCRIPCION:
            </div>

            <div class=" mb-3">                 
              APELLIDOS Y NOMBRES:    {{`${apellidoP} ${apellidoM} ${nombres}`}}
            </div>
            
            <div class=" mb-3">                 
              CEDULA DE IDENTIDAD:     {{`${ci_estudiante}`}}
            </div>

            <div class=" mb-3">                 
              CODIGO DE CARRERA:       {{`${codigo_carrera}`}}
            </div>

            <div class=" mb-3">                 
              NOMBRE DE CARRERA:         {{`${nombre_carrera}`}}
            </div>

            <div class=" mb-3">                 
              AÑO CURSADO:      {{`${anio_cursado}`}}
            </div>

            <!-- <div class=" mb-3">                 
              FECHA DE EMISION :         {{`${fecha_emision}`}}
            </div> -->

            <!-- <div>
            <button   class="btn btn-warning" @click="clickMe">                           
              GENERAR PDF : <i class="fa-solid fa-file-pdf"></i>
              </button>
            </div> -->

     </div>
  </div>


   <div class="row">
      <!-- <div class="col-lg-12 col-sm-12 offset-lg-2 align-center"> -->
        <!-- <div class="col-lg-12 col-sm-12 align-center"> -->
          <div class="table-responsive text-center">
              <table class="table table-bordered table-hover table-striped col-12" >
                  <thead class="pb-4 table-light">
                    <tr>
                      <th>
                        #
                      </th>
                      <th>
                        CODIGO CARRERA
                      </th>
                      <th>
                        CODIGO ASIGNATURA
                      </th> 
                   
                      <th>
                        NOMBRE DE ASIGNATURA
                      </th>
                      <th >
                        AÑO ASIGNADO
                      </th>
                                            
                      <!-- <th>
                        INS. DE APR.
                      </th>         
                      <th>
                        OBSERVACION
                      </th>-->                                            
                      <th>
                        ACCIONES
                      </th>
                    </tr></thead>
                  <tbody class="table-group-divider" id="contenido">
                    <tr v-for="materia,i  in ofertaMaterias" :key="materia">
                        <td>{{ i+1 }}</td>
                        <td>{{ materia.codigo_carrera }}</td>
                        <td>{{ materia.codigo_asignatura }}</td>
                        <td>{{ materia.nombre_asignatura }}</td>
                        <td>{{ materia.anio_asignado }}</td>
                        <!-- <td><inputcheckbox text="one" v-model="estado1" /></td> -->
                        <td><div class="form-check noselect">
                            <!-- <input class="form-check-input" type="checkbox" :id="id" :checked="estado1" :value="materia.codigo_asignatura" v-model="asignaturas" @change="addAsignatura()" /> -->
                            <input class="form-check-input" type="checkbox" :id="id" :checked="estado1" :value="materia.id" v-model="asignaturas" @change="addAsignatura()" />
                            <label class="form-check-label" :for="id"><slot /></label>
                          </div>
                        </td>
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
                        <!-- <router-link :to="{path:'/estudiante/ofertas/'+estudiante.ci_estudiante}" class="btn btn-warning">
                                INSCRIBIR<i class="fa-solid fa-school"></i>
                            </router-link>   -->
                    </tr>                    
                  </tbody>
              </table>
              <div>
                
              </div>
              

          <!-- </div> -->
      </div>
    </div>

    <div class="row">
      <div class="mb-3 mt-3 pa-3">
        <div class="d-grid col-6-mx-auto offset-md-6">        
                <button class="btn btn-warning" @click="guardarInscripcion">
                  <!-- <i class="fa-solid fa-user-plus"></i> -->
                  INSCRIBIR
                </button></div>    
      </div>
    </div>

  </div>
  <div v-else>
    <button @click="this.$router.back()" class="btn btn-warning col auto"><i class="fa-solid fa-arrow-left"></i>
      ATRAS
    </button>
    &nbsp; 
    <button  class="btn btn-warning col-6 text-center">
                    NO HAY OFERTA DE MATERIAS PARA EL ESTUDIANTE: {{`${apellidoP} ${apellidoM} ${nombres}`}}
    </button> 
  </div>
  

</template>

<script>
// @ is an alias to /src
//import {ref} from 'vue';
import axios from "axios";
import {show_alerta,sendRequest} from '../../funciones';
//para usar navegacion en la vista
import {  useRoute } from "vue-router";

//librerias para la exportacion en pdf
//import jsPDF from "jspdf";
//import autoTable from "jspdf-autotable";


//const router = useRouter(); 
//import {ref} from 'vue';
// const provincias = computed(()=>{
//         return this.provincias = this.getProvincias()
// })
//const provincias = ref([]);
 //const contador =ref(0);
export default {
  name: 'AprobadasEstudianteView',
  setup(){
    
  },
  data(){
    return {estudiantes:null,asignaturas:[],principal:'',
    ci_estudiante:'',nombres:'',apellidoP:'',apellidoM:'',codigo_carrera:'',nombre_carrera:'',anio_cursado:'',

    ofertaMaterias:[],estado1:false,
    url:'http://127.0.0.1:8000/administracion/obtenerAsignaturasNoCursadas'
  }
  },
  mounted(){
    const route =useRoute();    
     this.id = route.params.id;
      
     this.url = this.url + '/' +this.id+'/';
    this.getAsignaturasNoCursadas();
    //ruta de navegacion despues de la accion eliminar
    this.principal='/estudiantes';
  },
  methods:{    
    getAsignaturasNoCursadas(){
          axios.get(this.url)
            .then(            
                response =>(
                    this.ci_estudiante=response.data['estudiante']['ci_estudiante'],
                    this.nombres=response.data['estudiante']['nombres'],
                    this.apellidoP=response.data['estudiante']['apellidoP'],
                    this.apellidoM=response.data['estudiante']['apellidoM'],
                    this.codigo_carrera=response.data['estudiante']['codigo_carrera'],
                    this.nombre_carrera=response.data['estudiante']['nombre_carrera'],
                    this.anio_cursado=response.data['estudiante']['anio_cursado'],

                     //this.carreras=response.data['estudiante']
                     this.ofertaMaterias = response.data['oferta_materias']

                    //this.materias = response.data['datos']
                    
                )
            ).catch(error => { 
          console.log(error)
          show_alerta(error,'error')
        });
        //console.log(this.materias+'sss');
        console.log(this.url);
        console.log('aki'+this.ofertaMaterias);
            
    },
    addAsignatura(){
      console.log(this.asignaturas);
      // const parametros = {ci_estudiante:this.ci_estudiante,
      //                     asignaturas:this.asignaturas};
      // console.log(parametros);
      //sendRequest('PUT',parametros,this.url,'Estudiante Actualizado Exitosamente!',this.principal);                          

    //soporta asunc and await
    },async guardarInscripcion()
    {
      
      const parametros = {ci_estudiante:this.ci_estudiante,
                          ids_mallas:this.asignaturas};
                          //asignaturas:this.asignaturas};
      console.log(parametros);

       const ruta = 'http://127.0.0.1:8000/administracion/inscribirEstudiante/';

      //  axios({method:'POST',url:ruta,data:parametros}).then(resultado=>{
      //    console.log(resultado);
      //  }).catch(error => { 
      //   //const {data} =error.data
      //   console.log(error)
      //   console.log(error.response.data)
      //   show_alerta(error,'error')
      // });
      
      await sendRequest('POST',parametros,ruta,'Estudiante inscrito Exitosamente!',this.principal);       
      //router.push({ path: '/estudiantes' })
      this.$router.push('/estudiante/habilitados');
    }
  }
}
</script>