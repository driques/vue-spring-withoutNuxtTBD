<template>
    <div class="container" style="width:700px;">
        <h1>ELEMINAR DATOS</h1>
        <form>
            <div class="form-group">
                <label for="nombre">Nombre</label>
                <input type="text" class="form-control" maxlength="100" id="nombre" v-model="form.nombre" required>
            </div>
            <div class="form-group">
                <label for="descripcion">Descripción</label>
                <input type="text" class="form-control" maxlength="400" id="description" v-model="form.descrip"
                    required>
            </div>
            <button type="submit" class="btn btn-danger" v-on:click="eliminar()">ELEMINAR</button>
            <button type="submit" class="btn btn-dark" v-on:click="cancelar()">CANCELAR</button>
        </form>
    </div>

</template>

<script>

import axios from 'axios'

export default {
  name: 'BORRAR',
  data: function () {
    return {
      institucionId: '',
      form: {
        id: null,
        nombre: '',
        descrip: ''
      }
    }
  },
  methods: {
    cancelar () {
      this.$router.push('/instituciones')
    },
    eliminar () {
      const enviar = {
        id: this.institucionId,
        token: this.form.token
      }
      axios.delete('http://localhost:3000/instituciones/' + this.institucionId, { headers: enviar })
        .then(datos => {
          console.log(datos)
          this.makeToast('BORRADO', 'Institucion eleminada', 'danger')
          this.$router.push('/instituciones')
        }).catch(error => {
          console.log(error)
          this.makeToast('Error', 'No se pudo agregar la institucion', 'error')
        })
    },
    makeToast (titulo, texto, tipo) {
      this.toastCount++
      this.$bvToast.toast(texto, {
        title: titulo,
        variant: tipo,
        autoHideDelay: 5000,
        appendToast: true
      })
    }
  },

  mounted: function () {
    this.institucionId = this.$route.params.id
    const endpointInstitucion = 'http://localhost:3000/instituciones/' + this.institucionId
    axios.get(endpointInstitucion).then(
      response => {
        this.form.id = this.institucionId
        this.form.nombre = response.data.nombre
        this.form.descrip = response.data.descrip
        this.form.toke = localStorage.getItem('token')
      }
    )
  }
}
</script>
<style>

</style>
