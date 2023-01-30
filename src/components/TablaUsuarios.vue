<template>
  <div id="tabla-usuarios">
    <div v-if="!usuarios.length" class="alert alert-info" role="alert">
      No existen usuarios
    </div>
    <table class="table">
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Email</th>
          <th>Ciudad</th>
          <th>Teléfono</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="usuario in usuarios" :key="usuario.id">
          <td v-if='editando === usuario.id'>
            <input type="text" class="form-control" v-model="usuario.id"/>
          </td>
          <td v-else>{{ usuario.id }}</td>
          <td v-if='editando === usuario.id'>
            <input type="text" class="form-control" v-model="usuario.username"/>
          </td>
          <td v-else>{{ usuario.username }}</td>
          <td v-if='editando === usuario.id'>
            <input type="text" class="form-control" v-model="usuario.email"/>
          </td>
          <td v-else>{{ usuario.email }}</td>
          <td v-if='editando === usuario.id'>
            <input type="text" class="form-control" v-model="usuario.address.city"/>
          </td>
          <td v-else>{{ usuario.address.city }}</td>
          <td v-if='editando === usuario.id'>
            <input type="text" class="form-control" v-model="usuario.phone"/>
          </td>
          <td v-else>{{ usuario.phone }}</td>
          <td v-if='editando === usuario.id'>
            <button class="btn btn-success btn-sn" @click="guardarusuario(usuario)">Guardar</button>
            <button class="btn btn-danger ml-2" @click="cancelaredicion(usuario)">Cancelar</button>
          </td>
          <td v-else>
            <button class="btn btn-success btn-sn" @click="editarusuario(usuario)">Editar</button>
            <button class="btn btn-danger ml-2" @click="$emit('eliminar-usuario', usuario)">Eliminar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'TablaUsuarios',
  props: {
    usuarios: Array,
  },
  data(){
    return{
      editando: null,
    }
  },
  methods: {
    editarusuario(usuario){
      this.usuarioEditado = Object.assign({}, usuario);
      this.editando = usuario.id;
    },
    cancelaredicion(usuario){
      Object.assign(usuario, this.usuarioEditado);
      this.editando = null;
    },
    guardarusuario(usuario){
      if(!usuario.name.length || !usuario.username.length || !usuario.email.length || !usuario.address.city.length){
        return;
      }else{
      this.$emit('aztualizarusuario', usuario);
      this.editando = null;
      }
    }
  }
}
</script>







