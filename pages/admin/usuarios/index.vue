<template>
  <div>
    Listagem de usuários

    <v-data-table
      :headers="headers"
      :items="usuarios"
      sort-by="nome"
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar flat color="white">
          <v-toolbar-title>Usuários</v-toolbar-title>
          <v-spacer />
          <v-btn color="primary" dark class="mb-2" v-on="on" to="/admin/usuarios/incluir">Incluir</v-btn>
        </v-toolbar>
      </template>
      <template v-slot:item.action="{ item }">
        <v-icon
          small
          class="mr-2"
          @click="editar(item)"
        >
          mdi-pencil
        </v-icon>
        <v-icon
          small
          @click="excluir(item)"
        >
          mdi-delete
        </v-icon>
      </template>
      <template v-slot:no-data>
        Não há registros cadastrados!
      </template>
    </v-data-table>
  </div>
</template>

<script>
export default {
  data () {
    return {
      headers: [
        { text: 'Nome', value: 'nome' },
        { text: 'Apelido', value: 'apelido' },
        { text: 'Ações', value: 'action', sortable: false }
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
    }
  }
}
</script>
