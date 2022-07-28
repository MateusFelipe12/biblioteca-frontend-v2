<template>
  <v-container>
    <h1>Cadastro de Categorias</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col
            cols="3"
          >
            <v-text-field
              v-model="categoria.id"
              placeholder="Código"
              label="Código"
              :rules="rule"
              disabled
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col
          cols="7">
            <v-text-field
              v-model="categoria.nome"
              placeholder="Nome"
              label="Nome"
              :rules="rule"
              outlined
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      to="/categorias"
      color="red"
    >
      Cancelar
    </v-btn>
    <v-btn
      outlined
      @click="persistir"
      color="success"
    >
      Salvar
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroCategoriasPage',
  data () {
    return {
      categoria: {
        id: null,
        nome: null
      },
      rule: [
        v => !!v || `Campo obrigatorio`
      ],
      valid: false,
    }
  },

  created () {
    if (this.$route?.params?.id) {
      this.getById(this.$route.params.id)
    }
  },
  methods: {
    async persistir () {
     try {
      if(!this.valid) {
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }
      let categoria = {
          nome: this.categoria.nome
        };
      
      if(!this.categoria.id) {
        let response = await this.$axios.$post('http://localhost:3333/categorias', categoria);
        this.$router.push('/categorias');
        return this.$toast.success(`Categoria ${response.nome} criada com sucesso, id ${response.id}.`);
      }

      await this.$axios.$post(`http://localhost:3333/categorias/${this.categoria.id}`, categoria);
      this.$toast.success('Cadastro atualizado com sucesso!');
      return this.$router.push('/categorias');
       
       
     } catch (error) {
        this.$toast.error(`Ocorreu um erro ao realizar o cadastro, comunique o administrador`)
     }
    },
    async getById (id) {
      this.categoria = await this.$axios.$get(`http://localhost:3333/categorias/${id}`);
    }
  }
}
</script>