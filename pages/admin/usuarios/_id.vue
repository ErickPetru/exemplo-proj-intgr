<template>
  <v-container>
    <v-form
      ref="form"
      v-model="valid"
    >
      <v-card>
        <v-card-title>{{ modo }} usuário</v-card-title>
        <v-card-text>
          <v-text-field
            v-model="nome"
            :counter="50"
            :rules="nomeValidacao"
            label="Nome completo"
            required />

          <v-text-field
            v-model="apelido"
            :counter="15"
            :rules="apelidoValidacao"
            label="Apelido"
            required />

          <v-text-field
            v-model="email"
            :counter="100"
            :rules="emailValidacao"
            label="E-mail"
            autocomplete="new-password"
            required />

          <v-text-field
            type="password"
            v-model="senha"
            :counter="50"
            :rules="senhaValidacao"
            label="Senha"
            autocomplete="new-password"
            v-if="modo == 'Incluir'" />
        </v-card-text>
        <v-card-actions>
          <v-btn color="primary" @click="salvar" :disabled="!valid">
            <v-icon small class="mr-2">mdi-content-save</v-icon>
            Salvar
          </v-btn>
          <v-btn color="secondary" text @click="cancelar">
            <v-icon small class="mr-2">mdi-undo</v-icon>
            Cancelar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-form>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      valid: true,
      id: this.$route.params.id,
      modo: this.$route.params.id == 'incluir' ? 'Incluir' : 'Editar',
      nome: '',
      apelido: '',
      email: '',
      senha: '',
      nomeValidacao: [
        v => !!v || 'Nome completo é obrigatório',
        v => (v && v.length <= 50) || 'Nome completo deve ter no máximo 50 caracteres'
      ],
      apelidoValidacao: [
        v => !!v || 'Apelido é obrigatório',
        v => (v && v.length <= 15) || 'Apelido deve ter no máximo 15 caracteres'
      ],
      emailValidacao: [
        v => !!v || 'E-mail é obrigatório',
        v => (v && v.length <= 100) || 'E-mail deve ter no máximo 100 caracteres',
        v => /.+@.+\..+/.test(v) || 'E-mail deve ter um formato válido'
      ],
      senhaValidacao: [
        v => this.modo == 'Incluir' && (!!v || 'Senha é obrigatória'),
        v => this.modo == 'Incluir' && ((v && v.length <= 100) || 'Senha deve ter no máximo 50 caracteres')
      ]
    }
  },

  created () {
    const usuarios = this.$ls.get('usuarios')
    if (usuarios) {
      const usuario = usuarios.find(u => u.id == this.id)
      if (usuario) {
        this.nome = usuario.nome
        this.apelido = usuario.apelido
        this.email = usuario.email
      }
    }
  },

  methods: {
    gerarId () {
      return ([1e7]+-1e3+-4e3+-8e3+-1e11).replace(/[018]/g, c =>
        (c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c / 4).toString(16)
      );
    },

    salvar () {
      const usuarios = this.$ls.get('usuarios')
      if (!usuarios) usuarios = []

      if (this.modo == 'Incluir') {
        usuarios.push({
          id: this.gerarId(),
          nome: this.nome,
          apelido: this.apelido,
          email: this.email,
          senha: this.senha
        })
      } else {
        const i = usuarios.findIndex(u => u.id == this.id)
        usuarios[i].nome = this.nome
        usuarios[i].apelido = this.apelido
        usuarios[i].email = this.email
      }

      this.$ls.set('usuarios', usuarios)
      this.$router.push('/admin/usuarios')
    },

    cancelar () {
      this.$router.push('/admin/usuarios')
    }
  }
}
</script>
