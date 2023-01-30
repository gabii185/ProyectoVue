<template>
  <div id="formulario-usuario">
    <form @submit.prevent="enviarFormulario">
      <div class="container">
        <div class="row">
          <div class="col-md-4">
            <div class="form-group">
              <label>Nombre</label>
              <input
                ref="name"
                v-model="usuario.name"
                type="text"
                class="form-control"
                :class="{ 'is-invalid': procesando && nameInvalido }"
                @focus="resetEstado"
                @keypress="resetEstado"
              />
            </div>
          </div>
          <div class="col-md-4">
            <div class="form-group">
              <label>Email</label>
              <input
                ref="name"
                v-model="usuario.email"
                type="text"
                class="form-control"
                :class="{ 'is-invalid': procesando && emailInvalido }"
                @focus="resetEstado"
                @keypress="resetEstado"
              />
            </div>
          </div>
          <div class="col-md-4">
            <div class="form-group">
              <label>Telefono</label>
              <input
                ref="name"
                v-model="usuario.phone"
                type="text"
                class="form-control"
                :class="{ 'is-invalid': procesando && phoneInvalido }"
                @focus="resetEstado"
                @keypress="resetEstado"
              />
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-md-12">
            <div class="form-group">
              <button class="btn btn-primary">Alta Usuario</button>
            </div>
          </div>
        </div>
        <div class="container">
          <div class="row">
            <div class="col-md-12">
              <div
                v-if="error && procesando"
                class="alert alert-danger"
                role="alert"
              >
                Debes cubrir todos los campos
              </div>
              <div v-if="correct" class="alert alert-success" role="alert">
                Usuario dado de alta correcatmente
              </div>
            </div>
          </div>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "formulario-usuario",
  data() {
    return {
      procesando: false,
      correcto: false,
      error: false,
      usuario: {
        name: "",
        email: "",
        phone: "",
      },
    };
  },
  methods: {
    enviarForm(){
        this.procesando = true;
        this.resetEstado();
        if (this.nameInvalido || this.emailInvalido || this.phoneInvalido) {
            this.error = true;
            return;
        }
        this.$emit('crear-usuario', this.usuario);
        this.name.focus();
        this.error = false;
        this.correcto = true;
        this.procesando = false;

        this.usuario = {
            name: '',
            email: '',
            phone: ''
        }
    },
    resetEstado(){
        this.correcto = false;
        this.error = false;
    }
  },
  computed:{
    nameInvalido(){
        return this.usuario.name.length < 1;
    },
    emailInvalido(){
        return this.usuario.email.length < 1;
    },
    phoneInvalido(){
        return this.usuario.phone.length < 1;
    }
  }
};
</script>

<style>
</style>