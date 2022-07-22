<template>
  <v-container>
    <h1>Cadastro de autores</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col
            cols="3"
          >
            <v-text-field
              v-model="autor.id"
              placeholder="Código"
              label="Código"
              disabled
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col 
          cols="7">
            <v-text-field
              v-model="autor.nome"
              placeholder="Nome"
              :rules="rule"
              label="Nome"
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col 
          cols="7">
            <v-text-field
              v-model="autor.email"
              placeholder="Email"
              :rules="rule"
              label="Email"
              outlined
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      color="red"
      to="/autores"
    >
      Cancelar
    </v-btn>
    <v-btn
      outlined
      color="success"
      @click="persistir"
    >
      Salvar
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroAutoresPage',
  data () {
    return {
      autor: {
        id: null,
        nome: null,
        email: null,
      },
      rule: [
        v => !!v || `Campo obrigatorio`
      ]
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
        if (!this.valid) {
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }
        let autor = {
        nome: this.autor.nome,
        email: this.autor.email
        };
        if(!this.autor.id){
          let response = await this.$axios.$post('http://localhost:3333/autores', autor);
          location.reload();
          response = response.data.autor
          this.$toast.success(`Autor ${response.nome} criada com sucesso, id ${response.id}.`)
          return this.$router.push('/autores');
        }
        autor.id = this.autor.id
        await this.$axios.$post(`http://localhost:3333/autores`, autor);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/autores');
       
      } catch (error) {
         this.$toast.error(`Ocorreu um erro ao realizar o cadastro, comunique o administrador`)
      }
    },
    async getById (id) {
      let autor = await this.$axios.$get(`http://localhost:3333/autores/${id}`);
      this.autor = autor.data.autor;
    }
  }
}
</script>