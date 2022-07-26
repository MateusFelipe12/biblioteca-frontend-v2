<template>
  <v-container>
    <h1 style="color:aquamarine ;">Consulta de autores</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col
        cols="2">
          <v-btn
            outlined
            color="success"
            @click="getAutores"
          >
            Pesquisar
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            outlined
            color="blue"
            to="/autores/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="autores"
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
  name: 'ConsultaLivrosPage',
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
          text: 'Email',
          align: 'center',
          sortable: false,
          value: 'email'
        },
        { text: "", value: "actions" },
      ],
      autores: []
    }
  },
  created () {
    this.getAutores ()
  },

  methods: {
    async getAutores () {
      try {
        let autores = await this.$axios.$get('http://localhost:3333/autores');
        this.autores = autores.data.autores
      } catch (error) {
        this.$toast.error(`Ocorreu um erro de requisição, contate o administrador`)
      }
    },
    async deletar (autor) {
      try {
        if (confirm(`Deseja deletar o autor id ${autor.id} - ${autor.nome}?`)) {
        let response = await this.$axios.$post('http://localhost:3333/autores/deletar', { id: autor.id });
        this.$toast(response.message)
        this.getAutores();
      }
      } catch (error) {
        this.$toast.error(`Ocorreu um erro ao deletar o autor, contate o administrador`)
      }
    },
    async update (categoria) {
      this.$router.push({
        name: 'autores-cadastro',
        params: { id: categoria.id }
      });
    }
  }
}
</script>
