<template>
  <div class="container-fluid ">
    <!-- <div class="container-fluid text-center"> -->
    <div class="row">
      
      <div class="mb-3">
        <!-- <div class="d-flex justify-content-around ">         -->
        <div class="mb-3 col-lg-10 offset-1">

          <!-- <button class="btn btn-outline-success col-2">
          <i class="fa-solid fa-table"></i> <router-link to="/estudiante/curso-preparatorio" class="nav-link active"
            >ESTUDIANTES-PREPARATORIO</router-link>
        </button> &nbsp; -->

          <button class="btn btn-outline-success  col-2 ">
            <i class="fa-solid fa-user-plus"></i> <router-link to="/estudiante/create" class="nav-link active">NUEVO
              ESTUDIANTE</router-link>
          </button>

        </div>
      </div>
      <div class="fw-bold">
        <div class="mb-3 fs-4 text-center">                 
             PLANILLA DE ESTUDIANTES GENERAL:
        </div>
      </div>
    </div>
    <div class="row text-center">
      <!-- <div class="col-lg-12 col-sm-12 offset-lg-2 align-center"> -->
      <!-- <div class="col-lg-12 col-sm-12 align-center"> -->
      <div class="table-responsive">
        <table class="table table-bordered table-hover table-striped fixed small table-auto">
          <thead class="pb-4 table-light" v-if="estudiantes">
            <tr>
              <th class="col">
                #
              </th>
              <!-- <th class="d-none d-sm-block pb-4 col"> -->
              <th>
                C.I.
              </th>
              <th class="col-2">
                NOMBRE COMPLETO
              </th>
              <!-- <th>
                  APELLIDO-P.
                </th>
                <th class="col">
                  APELLIDO-M.
                </th> -->
              <th class="col-3">
                CARRERA
              </th>
              <th class="col-1">
                TIPO INGRESO
              </th>
              <th class="col-1">
                AÑO INGRESO
              </th>
              <th class="col-1">
                N° ARCHIVO
              </th>
              <th class="col-1">
                AÑO CURSADO
              </th>
              <th class="col-2">
                OBS.1
              </th>
              <th class="col-2">
                OBS.2
              </th>
              <!-- <th class="col-1">
                        OBS.1
                      </th>
                      <th class="col-1">
                        OBS.2
                      </th>                       -->
              <th class="col-1">
                ESTADO
              </th>
              <th class="col-1">
                INSCRITO
              </th>
              <th class="col-2">
                ACCIONES
              </th>
            </tr>
          </thead>
          <div v-else>
            <img src="loading.gif" alt="iamgen">
          </div>

          <tbody class="table-group-divider" id="contenido">
            <tr v-for="estudiante, i  in estudiantes" :key="estudiante.ci_estudiante">
              <td>{{ i + 1 }}</td>
              <!-- <td class="d-none d-sm-block pb-4">{{ estudiante.ci_estudiante }}</td> -->

              <td v-if="estudiante.ci_especial">{{ `${estudiante.ci_estudiante}${estudiante.ci_especial}` }}</td>
              <td v-else>{{ `${estudiante.ci_estudiante}` }}</td>

              <td class="col-2">{{ `${estudiante.apellidoP} ${estudiante.apellidoM} ${estudiante.nombres} ` }}</td>
              <td class="col-3">{{ estudiante.nombre_carrera }}</td>
              <td>{{ estudiante.tipo_ingreso }}</td>

              <td>{{ estudiante.anio_ingreso }}</td>
              <!--  <td>{{ getCarrera(estudiante.id_carrera) }}</td> -->
              <td>{{ estudiante.numero_archivo }}</td>
              <td>{{ estudiante.anio_cursado }}</td>
              <td class="col-1">{{ estudiante.obs1 }}</td>
              <td class="col-1">{{ estudiante.obs2 }}</td>
              <!-- <td>{{ estudiante.obs1 }}</td>
                        <td>{{ estudiante.obs2 }}</td> -->
              <!-- <td>{{ estudiante.id_carrera }}</td> -->
              <td>{{ estudiante.estado }}</td>
              <td>{{ estudiante.inscrito_gestion }}</td>
              <td class="d-flex justify-content-center">
                <div class="d-flex mb-1 ">
                                    
                  <button class="btn btn-outline-success" @click="inscribirEstudianteSextoAnio(estudiante.ci_estudiante)" >
                    <i class="fa-solid fa-building-columns"></i>
                  </button>
                  <router-link :to="{ path: '/estudiante/historial-avance-general/' + estudiante.ci_estudiante }"
                  class="btn btn-outline-success">
                    <i class="fa-solid fa-passport"></i>
                  </router-link>
                  <router-link :to="{ path: '/estudiante/materias-cursadas/' + estudiante.ci_estudiante }"
                  class="btn btn-outline-info">
                    <i class="fa-solid fa-layer-group"></i>
                  </router-link>
                  <button class="btn btn-outline-success" @click="formularioA(estudiante.ci_estudiante)" >
                    <i class="fa-solid fa-bars"></i>
                  </button>
                </div>
                <div class="d-flex mb-1 ">
                  <router-link :to="{ path: '/estudiante/edit/' + estudiante.ci_estudiante }"
                    class="btn btn-outline-warning" >
                    <i class="fa-solid fa-user-pen"></i>
                  </router-link>
                  <router-link :to="{ path: '/estudiante/HistorialAnual/' + estudiante.ci_estudiante }"
                    class="btn btn-outline-success" >
                    <i class="fa-solid fa-book"></i>
                  </router-link>
                  
                  <!-- <button class="btn btn-outline-success" @click="certificadoCalificaciones(estudiante.ci_estudiante)" >
                    <i class="fa-solid fa-book"></i>
                  </button> -->
                  <!-- <button class="btn btn-outline-danger" @click="eliminar(estudiante.ci_estudiante, estudiante.nombres)">
            <i class="fa-solid fa-trash"></i>
          </button> -->
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <!-- <div class="d-flex mb-1 ">
                  <router-link :to="{ path: '/estudiante/materias-cursadas/' + estudiante.ci_estudiante }"
                    class="btn btn-outline-success" 
                    >                    
                    <i class="fa-solid fa-user-graduate"></i>
                  </router-link> &nbsp;

                  <button class="btn btn-outline-success" @click="formularioA(estudiante.ci_estudiante)" >                   
                    <i class="fa-regular fa-clipboard"></i>
                  </button>                                                      
                </div>
                <div class="d-flex">
                  <router-link :to="{ path: '/estudiante/edit/' + estudiante.ci_estudiante }" class="btn btn-outline-warning"  
                    >                    
                    <i class="fa-solid fa-edit"></i>
                  </router-link> &nbsp;
                  <button class="btn btn-outline-danger" @click="eliminar(estudiante.ci_estudiante, estudiante.nombres)" >
                    
                    
                    <i class="fa-solid fa-trash"></i>
                  </button>                  
                </div>                -->
    </div>
  </div>
  <!-- data-bs-toggle="tooltip"
                    data-bs-placement="top"
                    title="Eliminar" -->
</template>

<script>
// @ is an alias to /src
//import {ref} from 'vue';
import axios from "axios";
import { confirmar1, show_alerta } from '../../funciones';
import { historialAcademico, generarReporteInscripcionEgresados} from '../../reportes'
import jsPDF from "jspdf";
import autoTable from "jspdf-autotable";

//import {ref} from 'vue';
// const provincias = computed(()=>{
//         return this.provincias = this.getProvincias()
// })
//const provincias = ref([]);
//const contador =ref(0);
import { Tooltip } from 'bootstrap'
let BASE_URL = process.env.VUE_APP_BASE_URL;
export default {
  name: 'EstudianteView',
  data() {
    return {
      estudiantes: null, carreras: [], principal: '', message: '', BASE_URL: '',
      datos_estudiantes: {
        gestion: '',
        datos_estudiante: {},
        requisitos: {},
        numero_archivo: '',
        materias_cursadas: {}
      }
    }
  },
  mounted() {
    this.getEstudiantes();
    //this.BASE_URL=process.env.VUE_APP_BASE_URL;
    //this.getMateriasCursadas();
    //ruta de navegacion despues de la accion eliminar
    this.principal = '/estudiantes';
    new Tooltip(document.body, {
      selector: "[data-bs-toggle='tooltip']",
      html: true, trigger: 'hover'
    })
  },
  methods: {
    async inscribirEstudianteSextoAnio(ci){
      const url = BASE_URL+'/administracion/inscripcionParaDefensa/'+ci+'/';

      await axios.get(url).then(resultado=>{
      //const datos_estudiante = this.datos_estudiante_sexto_anio['estudiante'];
      //console.log(this.datos_estudiante_sexto_anio+'as');
      const datos = resultado.data;
      //const datos_estudiante = datos['estudiante'];
      const {estudiantes,fecha_emision,numero_archivo,numero_boleta}= datos;

      //console.log(this.datos_estudiante_sexto_anio);
      const modalidad_egreso=[];
      //const bodyE = `${modalidad_egreso} ${anio_actual}`;      

      const anio_actual = datos['anio_actual'];
      //console.log(anio_actual);
      modalidad_egreso.push([`DEFENSA DE GRADO GESTIÓN ${anio_actual}`,'PENDIENTE']);
      console.log(estudiantes);
      generarReporteInscripcionEgresados(modalidad_egreso, estudiantes, fecha_emision, numero_boleta, numero_archivo, anio_actual);
      this.$router.push('/estudiante/habilitados');
      });

      }
    ,
    async formularioA(id) {
      const url = BASE_URL + '/estudiantes/formularioAdmision/' + id + '/';
      await axios.get(url)
        .then(
          response => {
            //const datos_estudiante=response.data['datos_estudiante'];
            this.datos_estudiantes = response.data;
            //console.log(this.datos_estudiante.requisitos[0].requisito);
            //console.log(this.datos_estudiante);
            this.exportPDF(this.datos_estudiantes);
          }
        ).catch(error => {
          console.log(error)
          show_alerta(error, 'error')
        });
    },
    async certificadoCalificaciones(id) {

      const url = BASE_URL + '/estudiantes/obtenerCertificacionGestionAnterior/' + id + '/';
      await axios.get(url)
        .then(
          response => {
            //const datos_estudiante=response.data['datos_estudiante'];
            this.materias_cursadas = response.data;
            //console.log(this.datos_estudiante.requisitos[0].requisito);
            //console.log(this.datos_estudiante);
            console.log(this.materias_cursadas.estudiante);
            const datos = this.materias_cursadas;
            const datos_estudiante = datos.estudiante;
            const grado = datos.grado;
            const fecha_emision = datos.fecha_emision;
            const otros_datos = datos.datos;
            if (!response.data.message) {
              historialAcademico(datos_estudiante, grado, fecha_emision, otros_datos);
            } else {
              show_alerta('El estudiante no curso la gestión anterior(2023)', 'error')
            }

          }
        ).catch(error => {
          console.log(error)
          show_alerta(error, 'error')
        });
    },
    async getEstudiantes() {
      //await axios.get('http://127.0.0.1:8000/estudiantes/estudiantes/')
      await axios.get(BASE_URL + '/estudiantes/estudiantes/')
        .then(
          response => (
            this.estudiantes = response.data
          )
        ).catch(error => {
          console.log(error)
          show_alerta(error, 'error')
        });
      console.log(this.estudiantes);
      //console.log(this.estudiantes);
      //console.log('AS'+BASE_URL);
    }, eliminar(id, nombre) {
      //   for (let index = 0; index < 10; index++) {
      //     sendRequest('POST',{
      //     "id":999,
      //     "nombre_provincia": "TEST-PROVINCIAS",
      //     "id_departamento": null
      // },'http://127.0.0.1:8000/parametros/provincias/','ProvinciaS Eliminada');              
      //   }
      const ruta = 'estudiantes/estudiantes/' + id + '/';
      confirmar1(id, nombre, ruta, this.principal);
      //this.$router.push('/estudiantes')    
    },
    getCarrera(id) {
      //axios.get('http://127.0.0.1:8000/parametros/carreras/' + id + '/')
      axios.get(BASE_URL + '/parametros/carreras/' + id + '/')
        .then(
          response => (
            this.carreras[id] = response.data['nombre_carrera']
          )
        );
      return this.carreras[id]
    }, getMateriasCursadas(id) {
      axios.get(BASE_URL + '/estudiantes/obtenerAsignaturasCursadas/' + id + '/')
        .then(
          response => {

            if (!response.data.message) {
              //console.log('psando normal');
              //this.message = response.data.message,
              this.materias_cursadas = response.data

              // this.ci_estudiante = response.data['estudiante']['ci_estudiante'],
              // this.nombres = response.data['estudiante']['nombres'],
              // this.apellidoP = response.data['estudiante']['apellidoP'],
              // this.apellidoM = response.data['estudiante']['apellidoM'],
              // this.numero_registro = response.data['estudiante']['numero_registro'],
              // this.nombre_carrera = response.data['estudiante']['nombre_carrera'],

              // this.fecha_emision = response.data['fecha_emision'],

              // this.materias = this.sortGestion(response.data['datos'])





              //this.materias = response.data['datos']
            } else {
              this.message = response.data.message
              console.log('aki esta el error' + response.data.message);
            }


            console.log(response.data);
          }
        ).catch(error => {
          console.log(error)
          show_alerta(error, 'error')
        });
    },
    formatDate(cadena) {
      console.log('ss' + cadena + 'esto');
      // const date = new Date(cadena);  
      // return new Intl.DateTimeFormat('es-BO').format(date).toString();
      //if (cadena !== '' && cadena !== 'null' && cadena !== 'undefined' && cadena !== ' ') {
        if(cadena){
        console.log('entro?');
        let fecha = '';
        const datos = cadena.split('-');
        fecha = datos[2] + '/' + datos[1] + '/' + datos[0];
        return fecha;
      }
    },
    sortGestion(data) {
      data = data.sort((a, b) => {
        if (a.anio_cursado < b.anio_cursado) {
          return -1;
        }
      });
      console.log(data);
      return data;
    },
    certificadoCalificacioness(ci_estudiante) {
      //console.log(this.getMateriasCursadas(ci_estudiante));
      let gestion = '2023'
      this.getMateriasCursadas(ci_estudiante);
      const datos_estudiante = this.materias_cursadas;
      //console.log(this.materias_cursadas);
      historialAcademico(datos_estudiante,gestion);
    },
    async exportPDF(datos_estudiante) {
      //first try
      //parameters:orientation,unit,format
      // const doc = new jsPDF('p', 'pt', 'A4');


      //   let pdfName = 'test';                     
      //   doc.text(`Hello Students:

      //     ${this.estudiantes[0].nombre_carrera}                   

      //     `, 10, 100);
      //   doc.save(pdfName + '.pdf');

      //second try
      //const doc = new jsPDF({unit: 'px'});
      const doc = new jsPDF({ orientation: 'p', unit: 'px', format: 'letter' });
      //const doc = new jsPDF('l','px,','letter');
      doc.setFontSize(12);
      //let setY=15;

      // this.estudiantes.forEach(element => {
      //   doc.text(`
      //     ${element.nombres}                                         
      //     `, 10, setY);
      //     setY+=15;
      // });

      //EJEMPLO QUE ESTA AL FINAL DE COMO PASAR LOS HEADERS Y BODY
      //const headers = [['Name', 'Email', 'Country']];
      //const headers1 = [['Nro','nombres', 'apellidoP', 'apellidoM','celular','nombre_carrera','estado']];  // 注意有兩層[]
      // const body = [
      //     ['David', 'david@example.com', 'Sweden'],
      //     ['Castille', 'castille@example.com', 'Spain']
      // ]

      //SEGUNDA FORMA CON EL RELLENADO DE UN FORMATO SOLICITADO ATRAVES DE ESTRUCTURAS DE DATOS, LA PRIMERA A TRAVES DE UN TAG HTML
      // const resultado = [];
      // const encabezado = [];

      // for (var i = 0; i < this.estudiantes.length; i += 1) {
      //   console.log(this.estudiantes[i]);
      //   //resultado.push(this.estudiantes[i])
      //    if(i==0)
      //    {
      //      encabezado.push({nombres:`${this.estudiantes[i].apellidoP} ${this.estudiantes[i].apellidoM} ${this.estudiantes[i].nombres} `,ci_estudiante:this.estudiantes[i].ci_estudiante,
      //                      nombre_carrera:this.estudiantes[i].nombre_carrera});
      //       console.log(encabezado);
      //    }
      //   resultado.push([i+1,this.estudiantes[i].nombres,this.estudiantes[i].apellidoP,this.estudiantes[i].apellidoM,this.estudiantes[i].celular,
      //   this.estudiantes[i].nombre_carrera,this.estudiantes[i].estado])
      //result.push(Object.assign({}, data));
      // }

      //UNNECESARY PIECE OF CODE 
      // let otro=[];
      //   this.estudiantes.forEach(element => {
      //     let jsonsito = {
      //       nombre:element.nombres,
      //       apellidoP:element.apellidoP,
      //       apellidoM:element.apellidoM
      //     }
      //     otro.push(jsonsito);

      //   doc.text(`
      //     ${element.nombres}                                         
      //     `, 10, setY);
      //     setY+=15;
      // });


      //numero de pagina
      //  const pageNumber=3;
      //  for (let i = 0; i < pageNumber; i++) {
      //     doc.setPage(i);
      //     let pageCurrent = doc.internal.getCurrentPageInfo().pageNumber; //現在這頁
      //     doc.setFontSize(12); //設定字體大小(optional)
      //     doc.text(
      //       `${pageCurrent} / ${pageNumber}`,
      //       190,
      //       doc.internal.pageSize.height - 10,
      //       {align: 'left'}
      //     );
      //   } 

      //<!--  -->
      //RELLENADO DE FORMA INICIAL()
      //RELLENADO DE DATOS DEL ESTUDIANTE:
      //INICIO PRIMERA FORMA



      const headers = [['N°', 'REQUISITOS DE ADMISIÓN', 'CUMPLE']];
      let requirements = [];
      const requisitos = datos_estudiante.requisitos;
      for (let index = 0; index < requisitos.length; index++) {
        requirements.push([index + 1, requisitos[index].requisito])
      }
      // doc.text(`
      //   Primer comentario                                        
      //    `, 10, setY);

      let finalY = doc.lastAutoTable.finalY || 10

      //doc.addImage("https://picsum.photos/200", "JPEG", doc.internal.pageSize.width-80, finalY+20, 50, 50);

      //await doc.addImage("../../logotipo-unibol-quechua.png", "PNG", doc.internal.pageSize.width-80, finalY+20, 50, 50);
      //await doc.addImage("../../caracteristicas-bosques-tropicales.jpg", "JPG", 30, finalY+20, 50, 50);

      //<!-- -->
      //PRIMER EJEMPLO DE PRUEBA EXITOSO
      // doc.text(`
      //    HISTORIAL ACADEMICO DE AVANCE GENERAL                                                      
      //    `, 20, finalY);
      //                   //finalY+=25;    
      //    finalY+=20;    
      // //SETEAMOS EL TAMAÑO DE LETRA PARA COLOCAR LOS DATOS
      // doc.setFontSize(10);
      // doc.text(`                            
      //    APELLIDOS Y NOMBRES: ${this.apellidoP} ${this.apellidoM} ${this.nombres}                                  
      //    CEDULA DE IDENTIDAD: ${this.ci_estudiante}
      //    NRO DE REGISTRO: ${this.numero_registro}
      //    CARRERA: ${this.nombre_carrera}
      //    FECHA DE EMISION: ${this.fecha_emision}
      //    NIVEL DE FORMACION: ${this.grado}
      //    `,80, finalY);
      //    finalY+=35;     

      //INICIANDO LOS ENCABEZADOS Y FORMATO DE PRESENTACION
      await doc.addImage("../../ministerio.jpg", "JPG", 15, finalY + 5, 60, 60);
      await doc.addImage("../../logotipo-unibol-quechua.png", "PNG", doc.internal.pageSize.width - 65, finalY + 5, 50, 50);

      doc.setTextColor(10);
      doc.setFontSize(10).setFont(undefined, 'bold');
      doc.setTextColor(18, 73, 39);
      doc.text(`
                       UNIVERSIDAD INDÍGENA BOLIVIANA COMUNITARIA INTERCULTURAL PRODUCTIVA
                       UNIBOL QUECHUA "CASIMIRO HUANCA"
                       `, (doc.internal.pageSize.getWidth() / 2) - 20, finalY, null, null, "center");
      //finalY+=25;    
      //añadimos 20+50 por el tamaño de las imagenes
      finalY += 20;

      doc.setTextColor(100);
      doc.setFontSize(8).setFont(undefined, 'normal');
      doc.text(`
                       Decreto Supremo N° 29664 de 2 de agosto de 2008 - Decreto Supremo N° 3079 del 8 de febrero 2017
                       R.M. 505/2013 - R.M. 1300/2018"
                       `, (doc.internal.pageSize.getWidth() / 2) - 20, finalY, null, null, "center");
      //finalY+=25;    
      //añadimos 20+50 por el tamaño de las imagenes
      finalY += 20;

      doc.setTextColor(10);
      doc.setFontSize(8);
      doc.text(`
                       Tukuy sunquwan yahcyaninchikta, ruwayninchikta, yuyayninchikta kallpachaspa sumaq kawsayman kutina                       
                       `, (doc.internal.pageSize.getWidth() / 2) - 5, finalY, null, null, "center");
      //finalY+=25;    
      //añadimos 20+50 por el tamaño de las imagenes
      finalY += 15;

      doc.setTextColor(10);
      doc.setFontSize(12).setFont(undefined, 'bold');
      doc.text(`
                      FORMULARIO DE ADMISIÓN UNIBOL-QUECHUA
                      `, (doc.internal.pageSize.getWidth() / 2) - 40, finalY, null, null, "center");
      //finalY+=25;    
      //añadimos 20+50 por el tamaño de las imagenes
      finalY += 10;

      doc.setTextColor(10);
      doc.setFontSize(18).setFont(undefined, 'bold');
      doc.text(`
                      GESTIÓN ${datos_estudiante.gestion}
                      `, (doc.internal.pageSize.getWidth() / 2) - 40, finalY, null, null, "center");
      //finalY+=25;    
      //añadimos 20+50 por el tamaño de las imagenes
      finalY += 100;

      doc.setTextColor(10);
      doc.setFontSize(12).setFont(undefined, 'bold');
      doc.text(`
                      C.I.: ${datos_estudiante.datos_estudiante.ci_estudiante}
                      `, (doc.internal.pageSize.getWidth() / 2) + 120, finalY, null, null, "center");
      //finalY+=25;    
      //añadimos 20+50 por el tamaño de las imagenes
      finalY += 10;

      doc.setTextColor(10);
      doc.setFontSize(14).setFont(undefined, 'bold');
      doc.text(`
                      DATOS PERSONALES
                      `, -30, finalY, null, null, "left");
      //finalY+=25;    
      //añadimos 20+50 por el tamaño de las imagenes
      finalY += 15;

      doc.setTextColor(10);
      doc.setFontSize(11);
      doc.text(`
                       Apellidos y Nombres:                                  
                       Fecha de Nacimiento: 
                       Estado Civil: 
                       Género: 
                       Lugar de Nacimiento: 
                       Correo Electrónico: 
                       Teléfono:
                       `, -20, finalY);
      //finalY+=25;    
      //añadimos 20+50 por el tamaño de las imagenes
      //finalY+=35; 
      //SETEAMOS EL TAMAÑO DE LETRA PARA COLOCAR LOS DATOS
      //doc.setFontSize(9);                       
      const estudiante1 = datos_estudiante.datos_estudiante;
      let estado = '';
      let genero = '';
      let email = '';
      let celular = '';
      let nacimiento = '';
      let fecha_nac = '';
      if (estudiante1.estado_civil) {
        estado = estudiante1.estado_civil.toUpperCase();
      } else {
        estado = '';
      }

      if (estudiante1.genero === 'M') {
        genero = 'MASCULINO';
      } else {
        genero = 'FEMENINO';
      }

      if (estudiante1.email) {
        email = estudiante1.email.toUpperCase();
      } else {
        email = '';
      }

      if (estudiante1.celular > 0) {
        celular = estudiante1.celular;
      } else {
        celular = '';
      }
      if (estudiante1.prov_nacimiento) {
        nacimiento = estudiante1.prov_nacimiento.toUpperCase();
      }
      
      fecha_nac = estudiante1.fecha_nacimiento?this.formatDate(estudiante1.fecha_nacimiento) : '';
      
      console.log(fecha_nac);
      console.log(fecha_nac + 'asi');

      doc.setTextColor(100);
      doc.setFontSize(11);
      doc.text(`
                       ${estudiante1.apellidoP} ${estudiante1.apellidoM} ${estudiante1.nombres}                         
                       ${fecha_nac}
                       ${estado}                                
                       ${genero}
                       ${nacimiento}
                       ${email}
                       ${celular}
                       `, (doc.internal.pageSize.getWidth() / 2) - 130, finalY);



      finalY += 75;

      doc.setTextColor(10);
      doc.setFontSize(14).setFont(undefined, 'bold');
      doc.text(`
                      DATOS DE ORGANIZACIÓN
                        `, -30, finalY, null, null, "left");
      //finalY+=25;    
      //añadimos 20+50 por el tamaño de las imagenes
      finalY += 15;

      doc.setTextColor(10);
      doc.setFontSize(11);
      doc.text(`                                                       
                        -Matriz: 
                        -Regional: 
                        -Comunidad: 
                        Lengua que habla:                        
                        `, -20, finalY);

      let matriz = '';
      let regional = '';
      let comunidad = '';
      let idioma = '';
      if (estudiante1.organizacion_matriz) {
        matriz = estudiante1.organizacion_matriz.toUpperCase();
      }
      if (estudiante1.organizacion_regional) {
        regional = estudiante1.organizacion_regional.toUpperCase();
      }
      if (estudiante1.comunidad_sindicato) {
        comunidad = estudiante1.comunidad_sindicato.toUpperCase();
      }
      if (estudiante1.idioma_nativo) {
        idioma = estudiante1.idioma_nativo;
      }


      doc.setTextColor(100);
      doc.setFontSize(11).setFont(undefined, 'bold');
      doc.text(`                                        
                       ${matriz}                                
                       ${regional}
                       ${comunidad}
                       ${idioma}                       
                       `, (doc.internal.pageSize.getWidth() / 2) - 130, finalY);

      finalY += 55;


      doc.setTextColor(10);
      doc.setFontSize(14).setFont(undefined, 'bold');
      doc.text(`
                      CARRERA DE INGRESO
                        `, -30, finalY, null, null, "left");
      //finalY+=25;    
      //añadimos 20+50 por el tamaño de las imagenes
      finalY += 20;

      doc.setTextColor(10);
      doc.setFontSize(11).setFont(undefined, 'bold');
      doc.text(`
                         ${estudiante1.nombre_carrera}
                        `, (doc.internal.pageSize.getWidth() / 2) - 20, finalY, null, null, "center");
      //finalY+=25;    
      //añadimos 20+50 por el tamaño de las imagenes
      finalY += 10;

      //const requisitos= datos_estudiante.requisitos;

      autoTable(doc, {
        //QUITANDO ESPACIO
        //startY: finalY + 20,               
        startY: finalY + 10,
        head: headers,
        body: requirements,
        //theme:'grid',theme:'striped',theme:'plain'
        theme: 'striped',
        tableLineColor: [0, 0, 0], tableLineWidth: 0.5,
        styles: { fontSize: 10, cellWidth: 'wrap', halign: 'justify' },
        bodyStyles: { lineWidth: 0.2, lineColor: [0, 0, 0] },
        headStyles: {
          fillColor: [255, 255, 255],
          textColor: [0, 0, 0],
          halign: 'center'
        },
        padding: 2,
        columnStyles: {
          0: { cellWidth: 50, halign: 'center' },
          1: { cellWidth: 299, halign: 'center' },
          2: { cellWidth: 50, halign: 'center' },
          // etc
        }
        //columnStyles:{color}                                          
      });



      finalY = doc.lastAutoTable.finalY
      finalY += 10;

      doc.setTextColor(10);
      doc.setFontSize(8).setFont(undefined, 'bold');
      doc.text(`
                    Arch: ${datos_estudiante.numero_archivo}              
                      `, -5, finalY);



      //doc.addImage("https://picsum.photos/200", "JPEG", 15, finalY+20, 10, 10);

      //const body = this.estudiantes;
      //const body =otro;

      //SEGUNDA FORMA FINALIZADA
      // autoTable(doc, {
      //   head: headers1,
      //   body:resultado,
      // });

      //doc.table(1, 1, this.generateData(100), headers1, { autoSize: true });

      //await doc.save(`${this.apellidoP} ${this.apellidoM} ${this.nombres}`);      
      await window.open(doc.output('bloburl'), '_blank');

      //var doc = new jsPDF('p', 'pt', 'A4');
      // margins = {
      //     top: 80,
      //     bottom: 60,
      //     left: 40,
      //     width: 522
      // };                                                                                                     
    }
  }
}
</script>
<style>
table,div {
    font-family: 'Montserrat', sans-serif;
    font-size: 12px;
  }
</style>