<template>
  <v-container>
    <h1 style="color:aquamarine ;">Consulta dos emprestimos dos Livros</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col cols="6">
           <v-text-field
          v-model="search"
          label="Search"
          placeholder="codigo"
          class="mx-3"
          :rules="rule"
        />
        </v-col>
        <v-col cols="1">
          <v-btn
            outlined
            color="grey"
            @click="livroEmprestado(search)"
          >
            Pesquisar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="livros"
      >
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
      livros: [],
      headers: [
        {
          text: 'Código', 
          align: 'center', 
          sortable: false, 
          value: 'id',
        },
        {
          text: 'prazo',
          align: 'center',
          sortable: false,
          value: 'prazo'
        },
        {
          text: 'devolucao',
          align: 'center',
          sortable: false,
          value: 'devolucao'
        },
        {
          text: 'id usuario', 
          align: 'center', 
          sortable: false, 
          value: 'idUsuario',
        },  
        { text: "", value: "actions" },
      ],
      rule: [
        v => !!v || `Campo obrigatorio`
      ]
    }
  },
  methods: {
    async livroEmprestado (idLivro) {
      try {
        this.livros = []
        let livro = await this.$axios.$post('http://localhost:3333/emprestimos/livro', {idLivro: idLivro})
        if(this.search){
          if(livro.error) {
            this.$toast.error(livro.message)
          }
          else{
            if(livro.data){
              livro.data.devolucao = 'Emprestado'
              this.livros.push(livro.data)
            } else{ 
                this.$toast(`o livro id ${idLivro} nao esta alocado em nenhum emprestimo`)
            }
          }
        } else{ this.$toast.error(`Preencha o campo de pesquisa`) }
      } catch (error) {
        this.$toast.error(`Ocorreu um erro no cadastro, contate o administrador`)
      }
    }
  }
}
</script>
