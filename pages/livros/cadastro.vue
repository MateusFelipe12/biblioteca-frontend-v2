<template>
  <v-container>
    <h1 style="color:aquamarine ;">Cadastro de Livros</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row no-gutters>
          <v-col
            cols="3"
          >
            <v-text-field
              v-model="livro.id"
              placeholder="Código"
              label="Código"
              disabled
              outlined
            />
          </v-col>
        </v-row>
        <v-row no-gutters>
          <v-col 
          cols="7">
            <v-text-field
              v-model="livro.titulo"
              placeholder="titulo"
              :rules="rule"
              label="titulo"
              outlined
            />
          </v-col>
        </v-row>
        <v-row no-gutters>
          <v-col 
          cols="7">
            <v-text-field
              v-model="livro.sinopse"
              placeholder="Sinopse"
              :rules="rule"
              label="Sinopse"
              outlined
            />
          </v-col>
        </v-row>
        <v-row no-gutters>
          <v-col
             cols="4"
          >
            <v-autocomplete
              v-model="livro.idAutor"
              :items="autores"
              outlined
              label="Autor"
              item-text="nome"
              item-value="id"
              style="margin-right:15px"
            ></v-autocomplete>
          </v-col>
             <v-col
            cols="3"
          >
            <v-autocomplete
              v-model="livro.idCategoria"
              :items="categorias"
              outlined
              label="Categoria"
              item-text="nome"
              item-value="id"
            ></v-autocomplete>
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      to="/livros"
    >
      Cancelar
    </v-btn>
    <v-btn
      outlined
      @click="persistir"
    >
      Salvar
    </v-btn>
  </v-container>
</template>


<script>
export default {
  name: 'CadastroLivrosPage',
  data () {
    return {
      livro: {
        id: null,
        titulo: null,
        sinopse: null,
        autor: null,
        categoria: null
      },
      rule: [
        v => !!v || `Campo obrigatorio`
      ],
      categorias: [],
      autores: []
    }
  },
  created () {
      this.getAutores();
      this.getCategorias();
      if (this.$route?.params?.id) {
      this.getById(this.$route.params.id)
    }
    },
  methods: {
    async persistir () {
      try {

        if(!this.valid){
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }
        let livro = {
          titulo: this.livro.titulo,
          sinopse: this.livro.sinopse,  
          idCategoria: this.livro.idCategoria,
          idAutor: this.livro.idAutor
        };

        if(!this.livro.id){
          let response = await this.$axios.$post('http://localhost:3333/livros', livro);
          response = response.data.livro
          this.$router.push('/livros')
          alert(`Livro ${response.titulo} criado com sucesso, id ${response.id}.`)
          return this.$router.push('/livros');
        }
        livro.id = this.livro.id
        await this.$axios.$post(`http://localhost:3333/livros`, livro);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/livros');
      } catch (error) {
          this.$toast.error(`Ocorreu um erro no cadastro, contate o administrador`)
      }
    },
    async getAutores () {
      let autores = await this.$axios.$get('http://localhost:3333/autores');
      this.autores = autores.data.autores
    },
    async getCategorias () {
      this.categorias = await this.$axios.$get('http://localhost:3333/categorias');
    },
    async getById (id) {
      let livro = await this.$axios.$get(`http://localhost:3333/livros/${id}`);
      this.livro = livro.data.livro
    }
  }
}
</script>