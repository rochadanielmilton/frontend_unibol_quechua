<template>
  <div class="row mt-3">
    <div class="col-md-6 offset-md-3">
      <div class="card">
        <div class="card-header bg-dark text-white text-center">
          REGISTRAR NUEVO IDIOMA
        </div>
        <div class="card-body">
          <form @submit="guardar">
            <div class="input-group mb-3">
              <span class="input-group-text"><i class="fa-solid fa-file-lines"></i>
              </span>
              <input type="text" v-model="nombre_idioma" id="nombre" class="form-control" maxlength="50" placeholder="Nombre del Idioma Originario" required>
            </div>
            <div class="d-grid col-6-mx-auto">
              <button class="btn btn-success">
                <i class="fa-solid fa-floppy-disk"></i> Guardar</button></div>            
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { show_alerta,sendRequest } from "../../funciones";
let BASE_URL=process.env.VUE_APP_BASE_URL;
export default {
  name: 'HomeView',
  data(){
    return {
      nombre_idioma:'',url:BASE_URL+'/parametros/idiomasOriginarios/'
    }
  },methods:{
    guardar(){
      event.preventDefault();
      if(this.nombre.trim()==='')
      {
        show_alerta('El idioma no puede ser vacio','warning','nombre_idioma');
      }else{
        const parametros={nombre_idioma:this.nombre.trim()};
        sendRequest('POST',parametros,this.url,'Idioma Originario Guardado Exitosamente!');
      }
    }
  }
}
</script>
