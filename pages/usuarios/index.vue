<template>
  <v-container>
    <h1 style="color: aquamarine ;">Consulta de Usuarios</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col
        cols="2">
          <v-btn
            outlined
            color="success"
            @click="getUsarios()"
          >
            Pesquisar
          </v-btn>
      </v-col>
        <v-col>
          <v-btn
            outlined
            color="blue"
            to="/usuarios/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="usuarios"
        class="elevation-1"
      >
        <template v-slot:item.actions="{ item }">
          <v-icon
            small
            class="mr-2"
            color="yellow"
            @click="update(item)"
          >
            mdi-pencil
          </v-icon>
          <v-icon
            small
            color="red"
            @click="deletar(item)"
          >
            mdi-delete
          </v-icon>
        </template>
      </v-data-table>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'ConsultaUsuariosPage',
  data () {
    return {
      headers: [
        {
          text: 'Código', 
          align: 'center', 
          sortable: false, 
          value: 'id',
        },
        {
          text: 'Nome',
          align: 'center',
          sortable: false,
          value: 'nome',
        },  
        {
          text: 'CPF/CNPJ',
          align: 'center',
          sortable: false,
          value: 'cpf_cnpj'
        },
        {
          text: 'Telefone', 
          align: 'center',
          sortable: false,
          value: 'telefone'
        },
        { text: "", value: "actions" },
      ],
      usuarios: []
    }
  },
  created () {
    this.getUsuarios ()
  },
  methods: {
    async getUsuarios () {
      try {
        let usuarios = await this.$axios.$get('http://localhost:3333/usuarios');
        this.usuarios = usuarios.data.usuario
      } catch (error) {
        this.$toast.error(`Ocorreu um erro ao carregar a pagina, contate o administrador`);
      }
    },
     async deletar (usuarios) {
      try {
        if (confirm(`Deseja deletar o usuarios id ${usuarios.id} - ${usuarios.nome}?`)) {
          let response = await this.$axios.$post('http://localhost:3333/usuarios/deletar', { id: usuarios.id });
          this.$toast(response.message)
          this.getUsuarios();
        } 
      } catch (error) {
        this.$toast.error(`Ocorreu um erro ao deletar, contate o administrador`);
      }
     },
       async update (usuario) {
        try {
          this.$router.push({
          name: 'usuarios-cadastro',
          params: { id: usuario.id }});
        } catch (error) {
           this.$toast.error(`Ocorreu um erro ao deletar, contate o administrador`);
        }
    }
  }
}
</script>
