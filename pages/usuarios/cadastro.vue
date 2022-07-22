<template>
  <v-container>
    <h1>Cadastro de Usuarios</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col
            cols="3"
          >
            <v-text-field
              v-model="usuario.id"
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
              v-model="usuario.nome"
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
              v-model="usuario.cpf_cnpj"
              placeholder="cpf/cnpj"
              :rules="rule"
              label="cpf/cnpj"
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col 
          cols="7">
            <v-text-field
              v-model="usuario.telefone"
              placeholder="Telefone"
              :rules="rule"
              label="Telefone"
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col 
          cols="7">
            <v-text-field
              v-model="usuario.email"
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
      to="/usuarios"
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
  name: 'CadastroUsuariosPage',
  data () {
    return {
      usuario: {
        id: null,
        nome: null,
        cpf_cnpj: null,
        email: null,
        telefone: null
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
        if(!this.valid){
          return this.$toast.warning('Ov formulário de cadastro não é válido!')
       }
        let usuario = {
          nome: this.usuario.nome,
          cpf_cnpj: this.usuario.cpf_cnpj,
          email: this.usuario.email,
          telefone: this.usuario.telefone,
        };
        if(!this.usuario.id){
          let response = await this.$axios.$post('http://localhost:3333/usuarios', usuario);
          location.reload();
          response = response.data.usuario
          this.$toast.success(`Categoria ${response.nome} criada com sucesso, id ${response.id}.`);
        }

        usuario.id = this.usuario.id
        await this.$axios.$post(`http://localhost:3333/usuarios`, usuario);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/usuarios');

      } catch (error) {
        this.$toast.error(`erro ao cadastrar, contate o administrador do sistema`)
      }
    },
    async getById (id) {
      let usuario = await this.$axios.$get(`http://localhost:3333/usuarios/${id}`);
      this.usuario = usuario.data.usuario
    }
  }
}
</script>