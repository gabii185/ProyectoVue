<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col-md-12 mt-2">
        <h1>GESTION CLIENTES</h1>
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <formulario-usuario @alta-usuario = 'postUsuario'/>
        <tabla-usuarios :usuarios="usuarios" @eliminar-usuario="deleteUsuario" @editar-usuario="editar_usuario"
          @actualizarusuario="putUsuario"/>
      </div>
    </div>
  </div>
</template>



<script>
import TablaUsuarios from "@/components/TablaUsuarios.vue";
import FormularioUsuario from "./components/FormularioUsuario.vue";

export default {
  name: "App",
  components: {
    FormularioUsuario,
    TablaUsuarios,
  },
  data() {
    return {
      usuarios: [],
    }
  },
  methods: {

    async postUsuario(usuario) {
      try {
        const response = await fetch('http://localhost:3000/usuarios',
          {
            method: "POST",
            body: JSON.stringify(usuario),
            headers: { "Content-type": "aplication/json; charset=UTF-8" },
          }
        );

        const usuarioAlta = await response.json();
        this.usuarios = [...this.usuarios,usuarioAlta]
      } catch (error) {
        console.error(error);
      }
    },

    async getUsuarios() {
      try {
        const response = await fetch("http://localhost:3000/usuarios");
        this.usuarios = await response.json();
      } catch (error) {
        console.log("error en getusuarios", error);
      }
    },
    async deleteUsuario(usuario) {
      try {
        await fetch(`http://localhost:3000/usuarios/${usuario.id}`, {
          method: "DELETE",
        });
        this.usuarios = this.usuarios.filter((u) => u.id !== usuario.id);
      } catch (error) {
        console.log(error);
      }
    },
    async putUsuario(id, usuario) {
      try {
        const response = await fetch(
          `http://localhost:3000/usuarios/${usuario.id}`,
          {
            method: "POST",
            body: JSON.stringify(usuario),
            headers: { "Content-type": "aplication/json; charset=UTF-8" },
          }
        );

        const usuarioaztualizado = await response.json();
        this.usuarios = this.usuarios.map((u) =>
          u.id === usuario.id ? usuarioaztualizado : u
        );
      } catch (error) {
        console.error(error);
      }
    },
  },
  mounted() {
    this.getUsuarios();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
