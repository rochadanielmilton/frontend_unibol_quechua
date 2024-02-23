<template>
<div class="container-fluid my-5" >
  <div class="row">
    <div class="mb-3 ">
      <!-- <div class="offset-7">         -->
              <!-- <button class="btn btn-success col-4 offset-8 ">                 
                <router-link to="/estudiante/create" class="nav-link active" href="#">Nuevo Estudiante</router-link>  <i class="fa-solid fa-user-plus"></i> 
               </button> -->
      <!-- </div>     -->

      <div class="mb-3 d-grid gap-2 d-md-block">
      
      <button class="btn btn-success col-lg-4 ">                 
        <router-link to="/estudiante/create" class="nav-link active" href="#">Nuevo Estudiante</router-link>  <i class="fa-solid fa-user-plus"></i> 
       </button>

      </div>

     </div>
  </div>
  <div class="row">
    <div class="mb-3 d-grid gap-2 d-md-block " id="proyeccion">
      <!-- <div class="offset-7">         -->
                <!-- <button class="btn btn-success  ">                 
                <router-link to="/estudiante/create" class="nav-link active" href="#">Nuevo Estudiante</router-link>  <i class="fa-solid fa-user-plus"></i> 
               </button> -->

                <button class="btn btn-warning  col-md-2 col-lg-2  ma-3" @click="update()" >                 
                Editar
                <i class="fa-solid fa-user-plus"></i> 
               </button>
               <button class="btn btn-danger   col-md-2  col-lg-2  ma-3" @click="eliminarSeleccionado()" >
                Eliminar
                <i class="fa-solid fa-trash"></i> 
               </button>
      <!-- </div>     -->
     </div>     
     
  </div>
  <!-- <BtnEditar :estudiantes="estudiantes"  @click="update">
      </BtnEditar> -->
   <div  class="row" >
      <!-- <div class="col-lg-12 col-sm-12 offset-lg-2 align-center"> -->
        <!-- <div class="col-lg-12 col-sm-12 align-center"> -->
          <!-- <BtnEditar :estudiantes="estudiantes" @click="actualizar">

          </BtnEditar> -->
        <div class="col-lg-12 offset-lg-0">          
          <div class="table-responsive">
            <DataTable  ref="table" id="datatable"  :data="estudiantes" :columns="columns" class="table table-bordered table-striped display fixed small" :options="{ select: true ,responsive: true,autoWidth:true,dom:'Bfrtip',
                            buttons : [{
                              extend : 'selected',
                              text : 'Edit',
                              name : 'edit'
                          }],pageLength:5,
                          //responsivePriority: 1,                                                   
                            columnDefs:[{
                               width:'40%',target:[6],
                              width:'10%',target:[7],
                            }],
                            language:{
                                      search:'Buscar',zeroRecord:'No hay registros que mostrar',
                                      info:'Mostrando desde _START_ a _END_ de _TOTAL_ registros',
                                      infoFiltered:'(Filtrados de _MAX_ registros)',
                                      paginate:{first:'Primero',previous:'Anterior',next:'Siguiente',last:'Ultimo'}
                            }}" :key="columns.length" >
                  <thead class="pb-4 table-light">
                    <!-- v-if="estudiantes" -->
                    <tr>
                      <th class="col">
                        #
                      </th>
                      <!-- <th class="d-none d-sm-block pb-4 col"> -->
                      <th>
                        C.I.
                      </th>
                      <th class="col-">
                        NOMBRES
                      </th>
                      <th>
                        APELLIDO-P.
                      </th>
                      <th class="col">
                        APELLIDO-M.
                      </th> 
                      <!-- <th class="col">
                        CELULAR
                      </th>  -->
                      <th class="col">
                        CARRERA
                      </th>
                      <th class="col">
                        AÑO DE INGRESO
                      </th>
                      <th class="col">
                        AÑO CURSADO
                      </th>
                      <!-- <th class="col-1">
                        OBS.1
                      </th>
                      <th class="col-1">
                        OBS.2
                      </th>                       -->
                      <th class="col">
                        ESTADO
                      </th>
                      <th class="col">
                        ACCIONES
                      </th>
                    </tr></thead>
                    <tbody>                      
                    </tbody>
                      
                    <!-- <div v-else>
                      <img src="loading.gif" alt="iamgen">
                    </div>                                 -->
              </DataTable>            
          <!-- </div> -->
      </div>
    </div>
    </div>        
  </div>

  


</template>

<script>
// @ is an alias to /src
//import BtnEditar from '../../components/BtnEditar';
import {confirmar1, show_alerta} from '../../funciones';
import {ref} from 'vue';
import axios from "axios";


import DataTable from 'datatables.net-vue3';
import DataTableLib from 'datatables.net-bs5';
import Select from "datatables.net-select";
import 'datatables.net-responsive-bs5';
//import 'datatables.net-select';

DataTable.use(DataTableLib);
DataTable.use(Select);


//import {ref} from 'vue';
// const provincias = computed(()=>{
//         return this.provincias = this.getProvincias()
// })
//const provincias = ref([]);
 //const contador =ref(0);
 let BASE_URL=process.env.VUE_APP_BASE_URL;
export default {
  components: {DataTable},
  name: 'EstudianteView',
  data(){
    return {
      estudiantes:null,carreras:[],principal:'',message:'',dt: null, seleccionado:false,
      columns:[
       {data:null,render:function(data,type,row,meta)
       {return `${meta.row+1}`}},
       {data:'ci_estudiante'},
       {data:'nombres'},

       {data:'apellidoP'},
       {data:'apellidoM'},
       //{data:'celular'},
       {data:'nombre_carrera'},
       {data:'anio_ingreso'},
       {data:'anio_cursado'},
       {data:'estado'},
       {
        data:'ci_estudiante',
        render:function(data){
          return `<button   data-id=${data} class="btn btn-danger" id="edit" @click="eliminar(${data.ci_estudiante},${data.nombres})">
                                <i class="fa-solid fa-trash"></i>
                  </button> &nbsp;
                  <router-link data-id=${data} :to="{path:'/estudiante/edit/'+ci_estudiante}" class="btn btn-warning">
                                <i class="fa-solid fa-edit"></i>
                            </router-link>
                  
                  `
        }
       },       
     ]     
    }
  },
  setup(){
        const table = ref(null)
      // ...
      return {
        table
      }
  },
  mounted(){
    this.getEstudiantes();
    //this.getMateriasCursadas();
    //ruta de navegacion despues de la accion eliminar
    this.principal='/estudiantes';
  },
  methods:{
    changeSelected(){
      let seleccion =this.seleccionado;
      this.table.dt.rows({selected:true}).every( function(){
        const row = this.data();
        console.log(row);
        if(row)
        {
          seleccion=!seleccion;
        }        
    });  
    this.seleccionado=seleccion;       

    },
    async update(){
      event.preventDefault();
        console.log('actualizando');
        let datos= this.estudiantes;
        let identificador='';
        //console.log(this.table.dt.rows({select:true})); 
         this.table.dt.rows({selected:true}).every( function(){
          //let idx = this.data().estudiantes.indexOf(this.data());
          
          //console.log(this.estudiantes.indexOf(this.data()));
          //TODO ESTO FUNCIONA MUY BIEN, EXCELENTE
          const row = this.data();
          console.log(row);
          // console.log(datos.indexOf(this.data()));
          let idx = datos.indexOf(this.data());
          let clave = datos[idx].ci_estudiante;
          //console.log(clave);
          identificador=clave;
          //this.$router.go('/about');
          //console.log(idx);
          //console.log(this.data().ci_estudiante);
          // let indice = this.estudiantes.map(e => e.ci_estudiante).indexOf('9675154')
          // console.log(indice);
          //  let indice = props.estudiantes.indexOf(this.data());
          //  let clave = props.estudiantes[indice].ci_estudiante;   
          //  console.log(clave);                 
         });
         if(identificador){
         this.$router.push('/estudiante/edit/'+identificador+'/');
         }else{
          const error='Debes Seleccionar Un Estudiante!';
          show_alerta(error,'info')
         }
        // console.log(this.table.dt.rows);
         
        // const tables = this.$refs.table;
        // console.log(tables.dt.rows({select:true}).every(()=>{
        // }));
    },
    async actualizar(){
      //console.log(props.estudiantes[1].nombres+'ahorari');
      await this.table.dt.rows({selected:true}).every(function (){
        //console.log(this.data());
        console.log(this.estudiantes+'ssd');
        //this.estudiantes.indexOf(this.data());
        //console.log(this.estudiantes[0].nombres);
        console.log('kicoz');
           //let indice = props.estudiantes.indexOf(this.data());
           //let clave = props.estudiantes[indice].ci_estudiante;   
           //console.log(clave);                 
         });
    },
    async getEstudiantes(){
           await axios.get(BASE_URL+'/estudiantes/estudiantes/')
            .then(            
                response =>(
                    this.estudiantes = response.data                        
                )
            );
        console.log(this.estudiantes);
    },eliminar(id,nombre=''){
    //   for (let index = 0; index < 10; index++) {
    //     sendRequest('POST',{
    //     "id":999,
    //     "nombre_provincia": "TEST-PROVINCIAS",
    //     "id_departamento": null
    // },'http://127.0.0.1:8000/parametros/provincias/','ProvinciaS Eliminada');              
    //   }
      const ruta = 'estudiantes/estudiantes/'+id+'/';
      confirmar1(id,nombre,ruta,this.principal);  
      //this.$router.push('/estudiantes')    
    },eliminarSeleccionado(){
      console.log('eliminando');
        let datos= this.estudiantes;
        let identificador='';
        let nombres='';
        //console.log(this.table.dt.rows({select:true})); 
         this.table.dt.rows({selected:true}).every( function(){
          //let idx = this.data().estudiantes.indexOf(this.data());
          
          //console.log(this.estudiantes.indexOf(this.data()));
          //TODO ESTO FUNCIONA MUY BIEN, EXCELENTE
          const row = this.data();
          console.log(row);
          // console.log(datos.indexOf(this.data()));
          let idx = datos.indexOf(this.data());
          let clave = datos[idx].ci_estudiante;
          
          //console.log(clave);
          identificador=clave;
          nombres =`${datos[idx].nombres} ${datos[idx].apellidoP} ${datos[idx].apellidoM}`;
          //this.$router.go('/about');
          //console.log(idx);
          //console.log(this.data().ci_estudiante);
          // let indice = this.estudiantes.map(e => e.ci_estudiante).indexOf('9675154')
          // console.log(indice);
          //  let indice = props.estudiantes.indexOf(this.data());
          //  let clave = props.estudiantes[indice].ci_estudiante;   
          //  console.log(clave);                 
         });
         if(identificador){
          const ruta = 'estudiantes/estudiantes/'+identificador+'/';
          confirmar1(identificador,nombres,ruta,this.principal);  
         //this.$router.push('/estudiante/edit/'+identificador+'/');
         }else{
          const error='Debes Seleccionar Un Estudiante!';
          show_alerta(error,'info')
         }
    },
    getCarrera(id){      
      axios.get(BASE_URL+'/parametros/carreras/'+id+'/')
            .then(            
                response =>(
                    this.carreras[id] = response.data['nombre_carrera']
                )
            );
          return this.carreras[id]
    },getMateriasCursadas(id){
          axios.get(BASE_URL+'/estudiantes/obtenerAsignaturasCursadas/'+id)
            .then(            
                response =>{
                    
                    if(!response.data.message){
                      console.log('psando normal');
                    //this.message = response.data.message,
                    this.ci_estudiante=response.data['estudiante']['ci_estudiante'],
                    this.nombres=response.data['estudiante']['nombres'],
                    this.apellidoP=response.data['estudiante']['apellidoP'],
                    this.apellidoM=response.data['estudiante']['apellidoM'],
                    this.numero_registro=response.data['estudiante']['numero_registro'],
                    this.nombre_carrera=response.data['estudiante']['nombre_carrera'],

                    this.fecha_emision=response.data['fecha_emision'],

                    this.materias = this.sortGestion(response.data['datos'])
                    
                    //this.materias = response.data['datos']
                     }else{
                      this.message = response.data.message
                      console.log('aki esta el error'+response.data.message);
                    }

                  
                    
                }
            ).catch(error => { 
          console.log(error)
          show_alerta(error,'error')
        });                         
    }
  }
}
</script>

<style>
/*@import 'bootstrap';*/
@import 'datatables.net-bs5';

/*@import 'datatables.net-dt';*/
@import 'datatables.net-responsive-dt';

table.dtr-inline.collapsed>tbody>tr>td.dtr-control, table.dataTable.dtr-inline.collapsed>tbody>tr>th.dtr-control {
    position: relative;
    padding-left: 30px;
    cursor: pointer;
}
table.dataTable.dtr-inline.collapsed>tbody>tr.parent>td.dtr-control:before, table.dataTable.dtr-inline.collapsed>tbody>tr.parent>th.dtr-control:before {
    content: "-";
    background-color: #d33333;
}
table.dataTable.dtr-inline.collapsed>tbody>tr>td.dtr-control:before, table.dataTable.dtr-inline.collapsed>tbody>tr>th.dtr-control:before {
    top: 33%;
    left: 5px;
    height: 1em;
    width: 1em;
    margin-top: -5px;
    display: block;
    position: absolute;
    color: white;
    border: .15em solid white;
    border-radius: 1em;
    box-shadow: 0 0 0.2em #444;
    box-sizing: content-box;
    text-align: center;
    text-indent: 0 !important;
    font-family: "Courier New",Courier,monospace;
    line-height: 1em;
    content: "+";
    background-color: #31b131;
    background-color: #74a2ff;
    
}
.box-buttons{
  display: flex;
  justify-content:space-around;
}

</style>
