<template>
  <v-container>
    <h1 style="color:aquamarine ;">Consulta de Categorias</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col
        cols="2">
          <v-btn
            outlined
            color="success"
            @click="getCategorias"
          >
            Pesquisar
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            outlined
            color="blue"
            to="/categorias/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="categorias"
        :items-per-page="10"
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
  name: 'ConsultaCategoriasPage',
  data () {
    return {
      headers: [
        {
          text: 'Código', //nome da coluna
          align: 'center', //alinhamento -center, end, start
          sortable: false, //se permite ordenação dos dados por essa coluna
          value: 'id', //é o dado que essa coluna vai receber
        },
        {
          text: 'Nome',
          align: 'center',
          sortable: false,
          value: 'nome',
        },
        { text: "", value: "actions" },
      ],
      categorias: []
    }
  },
  created () {
    this.getCategorias()
  },
  methods: {
    async getCategorias () {
      try{ 
       this.categorias = await this.$axios.$get('http://localhost:3333/categorias');
      } catch (error) {
        this.$toast.error(`Ocorreu um erro ao carregar a pagina, contate o administrador`)
      }
    },
    async deletar (categoria) {
      try {
        if (confirm(`Deseja deletar o categoria id ${categoria.id} - ${categoria.nome}?`)) {
          let response = await this.$axios.$post('http://localhost:3333/categorias/deletar', { id: categoria.id });
          this.$toast(response.message)
          this.getCategorias();
        }
      } catch (error) {
        this.$toast.error(`Ocorreu um erro ao deletar a categoria, contate o administrador`)
      }
    },
     async update (categoria) {
      this.$router.push({
        name: 'categorias-cadastro',
        params: { id: categoria.id }
      });
    }
  }
}
</script>