<template>
  <div>
    Incluir usuário

    <input v-model="nome" placeholder="Nome do usuário">
    <input v-model="apelido" placeholder="Apelido">
    <input v-model="senha" placeholder="Senha" type="password">

    <button @click="salvar">Salvar</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      nome: '',
      apelido: '',
      senha: ''
    }
  },

  methods: {
    salvar () {
      const dados = {
        nome: this.nome,
        apelido: this.apelido,
        senha: this.senha
      }

      let usuarios = this.$ls.get('usuarios')
      if (usuarios) {
        dados.id = usuarios.length + 1
        usuarios.push(dados)
      } else {
        dados.id = 1
        usuarios = [ dados ]
      }
      this.$ls.set('usuarios', usuarios)

      this.$router.push('/admin/usuarios')
    }
  }
}
</script>
