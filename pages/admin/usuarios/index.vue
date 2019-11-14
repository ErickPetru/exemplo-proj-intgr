<template>
  <v-container>
    <v-data-table
      :headers="headers"
      :items="usuarios"
      sort-by="nome"
      class="elevation-1"
      :hide-default-footer="true"
    >
      <template v-slot:top>
        <v-toolbar flat color="white">
          <v-toolbar-title>Usuários</v-toolbar-title>
          <v-spacer />
          <v-btn
            color="primary"
            to="/admin/usuarios/incluir"
            class="elevation-1"
            small
            fab
          >
            <v-icon>mdi-plus</v-icon>
          </v-btn>
        </v-toolbar>
      </template>
      <template v-slot:item.action="{ item }">
        <v-icon
          color="primary"
          class="mr-2"
          @click="editar(item)"
        >
          mdi-pencil
        </v-icon>
        <v-icon
          color="pink"
          @click="excluir(item)"
        >
          mdi-delete
        </v-icon>
      </template>
      <template v-slot:no-data>
        Não há registros cadastrados!
      </template>
    </v-data-table>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      headers: [
        { text: 'Nome completo', value: 'nome' },
        { text: 'Apelido', value: 'apelido' },
        { text: 'E-mail', value: 'email' },
        { text: 'Ações', value: 'action', sortable: false, width: 100 }
      ],

      usuarios: []
    }
  },

  created () {
    const usuarios = this.$ls.get('usuarios')
    if (usuarios) this.usuarios = usuarios
  },

  methods: {
    editar (item) {
      this.$router.push(`/admin/usuarios/${item.id}`)
    },

    excluir (item) {
      let usuarios = this.$ls.get('usuarios')
      usuarios = usuarios.filter(u => u.id != item.id)
      this.$ls.set('usuarios', usuarios)
      this.usuarios = usuarios
    }
  }
}
</script>
