<template>
  <div>
    Editar usuário

    <input v-model="nome" placeholder="Nome do usuário">
    <input v-model="apelido" placeholder="Apelido">

    <button @click="salvar">Salvar</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      id: this.$route.params.id,
      nome: '',
      apelido: ''
    }
  },

  created () {
    const usuarios = this.$ls.get('usuarios')
    if (usuarios) {
      const item = usuarios.find(r => r.id == this.id)

      if (item) {
        this.nome = item.nome
        this.apelido = item.apelido
      }
    }
  },

  methods: {
    salvar () {

    }
  }
}
</script>
