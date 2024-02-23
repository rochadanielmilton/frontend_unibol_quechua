<template>
    <div class="d-flex justify-content-around">                
      <!-- <button class="btn btn-warning"  :estudiantes="estudiantes" @click="Editar"> -->
      <button class="btn btn-warning editar"  :estudiantes="estudiantes" @click="Editar">
        Editar
      </button>        
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
        console.log(this.estudiantes);
      }   
    },
    mounted() {
      // UploadService.getFiles().then(response => {
      //   this.fileInfos = response.data;
      // });
    }
  };
  </script>
<style scoped>
.editar{
  display: inline;
}
</style>