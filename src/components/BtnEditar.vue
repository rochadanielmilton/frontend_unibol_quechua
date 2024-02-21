<template>
    <div>                
      <button class="btn btn-warning"  :estudiantes="estudiantes">
        Editar
      </button><i class="fa-solid fa-user"></i>        
    </div>
  </template>
  
  <script>
  //import UploadService from "../services/UploadFilesService";
  
  export default {
    props: {
      estudiantes:{
        type:Array
      }
    },
    name: "BtnEditar",
    data() {
      return {

      };
    },
    setup(props) {
    // setup() receives props as the first argument.
    console.log(props.estudiantes)
    },
    methods: {
      Editar()
      {
        console.log('hsata aki todo positivo');
        //console.log(props.estudiantes);
      }   
    },
    mounted() {
      // UploadService.getFiles().then(response => {
      //   this.fileInfos = response.data;
      // });
    }
  };
  </script>