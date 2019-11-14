<template>
  <v-layout
    column
    justify-center
    align-center
  >
    Bem-vindo!

    <p>
      Já somos {{usuariosCadastrados}} usuários!
    </p>
  </v-layout>
</template>

<script>
export default {
  data () {
    return {
      usuariosCadastrados: 0
    }
  },

  created () {
    const usuarios = this.$ls.get('usuarios')
    if (usuarios) this.usuariosCadastrados = usuarios.length
  }
}
</script>
