<template>
  <nav class="navbar" role="navigation" aria-label="main navigation">
    <div class="navbar-brand">
      <a class="navbar-item" href="/">
        <img src="../assets/psyduck.png" />
      </a>

      <a
        role="button"
        class="navbar-burger"
        aria-label="menu"
        aria-expanded="false"
        data-target="navbarBasicExample"
      >
        <span aria-hidden="true"></span>
        <span aria-hidden="true"></span>
        <span aria-hidden="true"></span>
      </a>
    </div>

    <div id="navbarBasicExample" class="navbar-menu">
      <div class="navbar-start">
        <a class="navbar-item"> Home </a>

        <a class="navbar-item"> Documentation </a>

        <div class="navbar-item has-dropdown is-hoverable">
          <a class="navbar-link"> More </a>

          <div class="navbar-dropdown">
            <a class="navbar-item"> About </a>
            <a class="navbar-item"> Jobs </a>
            <a class="navbar-item"> Contact </a>
            <hr class="navbar-divider" />
            <a class="navbar-item"> Report an issue </a>
          </div>
        </div>
      </div>

      <div class="navbar-end">
        <div class="navbar-item">
          <div class="search">
            <div class="control is-loading tamanho">
              <form id="form" @submit.prevent="enviarFormulario">
                <input
                  id="code"
                  type="text"
                  placeholder="Buscar"
                  class="input is-link is-rounded control is-loading tamanhoInp"
                  v-model="busca"
                />
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  data() {
    return {
      busca: "",
    };
  },
  props:{
    metodoPai: {
      type: Function,
      required: true,
    },
  },
  methods: {
    enviarFormulario() {
      if (this.busca.length >= 3) {
        this.metodoPai()
        console.log('Enviando formulário com busca:', this.busca);
        this.$emit('enviar-para-pai', this.busca);
      }
    },
  },
  
};
</script>

<style scoped>
.tamanho {
  max-width: 250px;
  margin-left: 38%;
}
.tamanhoInp {
  text-indent: 5%;
}
</style>