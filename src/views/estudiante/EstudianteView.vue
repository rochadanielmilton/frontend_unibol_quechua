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
    </div>
    <div class="row text-center">
      <!-- <div class="col-lg-12 col-sm-12 offset-lg-2 align-center"> -->
      <!-- <div class="col-lg-12 col-sm-12 align-center"> -->
      <div class="table-responsive">
        <table class="table table-bordered table-hover table-striped fixed small responsive">
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

          <tbody class="table-group-divider" id="contenido">
            <tr v-for="estudiante, i  in estudiantes" :key="estudiante.ci_estudiante">
              <td>{{ i + 1 }}</td>
              <!-- <td class="d-none d-sm-block pb-4">{{ estudiante.ci_estudiante }}</td> -->

              <td v-if="estudiante.ci_especial">{{ `${estudiante.ci_estudiante}${estudiante.ci_especial}` }}</td>
              <td v-else>{{ `${estudiante.ci_estudiante}` }}</td>

              <td class="col-2">{{ `${estudiante.nombres} ${estudiante.apellidoP} ${estudiante.apellidoM}` }}</td>
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
                <router-link :to="{ path: '/estudiante/materias-cursadas/' + estudiante.ci_estudiante }"
                  class="btn btn-outline-success me-1">
                  <i class="fa-solid fa-user-graduate"></i>
                </router-link>
                <router-link :to="{ path: '/estudiante/edit/' + estudiante.ci_estudiante }"
                  class="btn btn-outline-warning me-1">
                  <i class="fa-solid fa-edit"></i>
                </router-link>
                <!-- <button class="btn btn-outline-danger" @click="eliminar(estudiante.ci_estudiante, estudiante.nombres)">
            <i class="fa-solid fa-trash"></i>
          </button> -->
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="d-flex mb-1 ">
        <router-link :to="{ path: '/estudiante/materias-cursadas/' + estudiante.ci_estudiante }"
          class="btn btn-outline-success">
          <!-- data-bs-toggle="tooltip"
                    data-bs-placement="top"
                    title="Historial Academico" -->
          <i class="fa-solid fa-user-graduate"></i>
        </router-link> &nbsp;

        <button class="btn btn-outline-success" @click="formularioA(estudiante.ci_estudiante)">
          <i class="fa-regular fa-clipboard"></i>
        </button>
      </div>
      <div class="d-flex">
        <router-link :to="{ path: '/estudiante/edit/' + estudiante.ci_estudiante }" class="btn btn-outline-warning">
          <!-- data-bs-toggle="tooltip"
                    data-bs-placement="top"
                    title="Editar" -->
          <i class="fa-solid fa-edit"></i>
        </router-link> &nbsp;
        <button class="btn btn-outline-danger" @click="eliminar(estudiante.ci_estudiante, estudiante.nombres)">
          <!-- data-bs-toggle="tooltip"
                    data-bs-placement="top"
                    title="Eliminar" -->
          <i class="fa-solid fa-trash"></i>
        </button>
        <!-- FUNCIONA PERO NO ESTA ESTILIZADO -->
        <!-- <b-button v-b-tooltip.hover title="Tooltip directive content">
                              Hover Me
                            </b-button> -->
      </div>
      <!-- </td>
      </tr>
      </tbody>
      </table> -->

      <!-- </div> -->
    </div>
  </div>
  <!-- </div> -->
</div></template>

<script>
// @ is an alias to /src
//import {ref} from 'vue';
import axios from "axios";
import { confirmar1, show_alerta } from '../../funciones';
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
      datos_estudiante: {
        gestion: '',
        datos_estudiante: {},
        requisitos: {},
        numero_archivo: ''
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
    })
  },
  methods: {
    async formularioA(id) {
      const url = BASE_URL + '/estudiantes/formularioAdmision/' + id + '/';
      await axios.get(url)
        .then(
          response => {
            //const datos_estudiante=response.data['datos_estudiante'];
            this.datos_estudiante = response.data;
            //console.log(this.datos_estudiante.requisitos[0].requisito);
            //console.log(this.datos_estudiante);
            this.exportPDF(this.datos_estudiante);
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
      axios.get(BASE_URL + '/estudiantes/obtenerAsignaturasCursadas/' + id)
        .then(
          response => {

            if (!response.data.message) {
              console.log('psando normal');
              //this.message = response.data.message,
              this.ci_estudiante = response.data['estudiante']['ci_estudiante'],
                this.nombres = response.data['estudiante']['nombres'],
                this.apellidoP = response.data['estudiante']['apellidoP'],
                this.apellidoM = response.data['estudiante']['apellidoM'],
                this.numero_registro = response.data['estudiante']['numero_registro'],
                this.nombre_carrera = response.data['estudiante']['nombre_carrera'],

                this.fecha_emision = response.data['fecha_emision'],

                this.materias = this.sortGestion(response.data['datos'])

              //this.materias = response.data['datos']
            } else {
              this.message = response.data.message
              console.log('aki esta el error' + response.data.message);
            }



          }
        ).catch(error => {
          console.log(error)
          show_alerta(error, 'error')
        });
    },
    formatDate(cadena) {
      const date = new Date(cadena);
      return new Intl.DateTimeFormat('es-BO').format(date);
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
      doc.setFontSize(12);
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
      const estudiante = datos_estudiante.datos_estudiante;


      doc.setTextColor(100);
      doc.setFontSize(12);
      doc.text(`
                       ${estudiante.apellidoP} ${estudiante.apellidoM} ${estudiante.nombres}                         
                       ${this.formatDate(estudiante.fecha_nacimiento)}                       
                       ${estudiante.estado_civil}                                
                       ${estudiante.genero}
                       ${estudiante.prov_nacimiento}
                       ${estudiante.email}
                       ${estudiante.celular}
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
      doc.setFontSize(12);
      doc.text(`                                                       
                        -Matriz: 
                        -Regional: 
                        -Comunidad: 
                        Lengua que habla:                        
                        `, -20, finalY);

      doc.setTextColor(100);
      doc.text(`                                        
                       ${estudiante.organizacion_matriz}                                
                       ${estudiante.orgranizacion_regional}
                       ${estudiante.comunidad_sindicato}
                       ${estudiante.idioma_nativo}                       
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
                         ${estudiante.nombre_carrera}
                        `, (doc.internal.pageSize.getWidth() / 2) - 20, finalY, null, null, "center");
      //finalY+=25;    
      //añadimos 20+50 por el tamaño de las imagenes
      finalY += 10;

      //const requisitos= datos_estudiante.requisitos;

      autoTable(doc, {
        //QUITANDO ESPACIO
        //startY: finalY + 20,               
        startY: finalY + 20,
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
        padding: 8
        //columnStyles:{color}

      });



      finalY = doc.lastAutoTable.finalY
      finalY += 30;



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
/* body {
    font-size: .875rem;
    line-height: 1.25rem;
} */
</style>