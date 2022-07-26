<template>
  <v-container>
    <h1 style="color:aquamarine ;">Consulta de Livros</h1>
    <hr>
    <v-container>
      <v-row>
         <v-col cols="6">
           <v-text-field
          v-model="search"
          label="Search"
          class="mx-4"
        />
        </v-col>
        <v-col cols="3">
          <v-btn
            outlined
            color="blue"
            to="/livros/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            outlined
            color="white"
            to="/livros/consultar"
          >
            Consultar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="livros"
        class="elevation-1"
        item-key="Titulo"
        :search="search"
      >
      <template v-slot:item.actions="{ item }">
        <v-icon
          small
          color="yellow"
          class="mr-2"
          @click="update(item)"
        >
          mdi-pencil
        </v-icon>
        <v-icon
          small
          color="red"
          class="mr-2"
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
      search: '',
      calories: '',
      livros: [],
      headers: [
        {
          text: 'Código', 
          align: 'center', 
          sortable: false, 
          value: 'id',
        },
        {
          text: 'Titulo',
          align: 'center',
          sortable: false,
          value: 'titulo'
        },
        {
          text: 'Sinopse',
          align: 'center',
          sortable: false,
          value: 'sinopse'
        },
        {
          text: 'Autor', 
          align: 'center', 
          sortable: false, 
          value: 'idAutor',
        },  
        {
          text: 'Categoria', 
          align: 'center', 
          sortable: false, 
          value: 'idCategoria',

        },  
        {
          text: 'Emprestado', 
          align: 'center', 
          sortable: false, 
          value: 'emprestado',
        },
        { text: "", value: "actions" },
      ]
    }
  },
  created () {
    this.getLivros ()
  },
  methods: {
    async getLivros () {
     try {
        let livros = await this.$axios.$get('http://localhost:3333/livros');
        this.livros = livros.data.livros
     } catch (error) {
      this.$toast.error(`Ocorreu um erro ao carregar a pagina, contate o administrador`)
     }
    },

    async deletar (livros) {
      if (confirm(`Deseja deletar o livro id ${livros.id} - ${livros.titulo}?`)) {
        let response = await this.$axios.$post('http://localhost:3333/livros/deletar', { id: livros.id });
        this.$toast(response.message)
        this.getLivros();
      }
     },
    async update (livro) {
      this.$router.push({
        name: 'livros-cadastro',
        params: { id: livro.id }
      });
    }
  }
}
</script>
