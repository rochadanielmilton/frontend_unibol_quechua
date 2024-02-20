<template>
    <div>                
      <button class="btn btn-warning"  @click="Editar">
        Editar
      </button><i class="fa-solid fa-user"></i>
        
    </div>
  </template>
  
  <script>
  //import UploadService from "../services/UploadFilesService";
  
  export default {
    name: "BtnEditar",
    data() {
      return {
        
      };
    },
    methods: {
      Editar()
      {
        console.log('hsata aki todo positivo');
      }   
    },
    mounted() {
      // UploadService.getFiles().then(response => {
      //   this.fileInfos = response.data;
      // });
    }
  };
  </script>