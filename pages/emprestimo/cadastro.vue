<template>
  <v-container>
    <h1>Cadastro de emprestimos</h1>
    <hr>
    <v-container>
       <v-form v-model="valid">
        <template>

      <!-- calendario -->
          <v-row>
            <v-col>
              <v-menu
                ref="menu"
                v-model="menu"
                :close-on-content-click="false"
                transition="scale-transition"
                offset-y
                min-width="auto"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-text-field
                    :rules="rule"
                    v-model="date"
                    label="Prazo de devoluçao"
                    prepend-icon="mdi-calendar"
                    readonly
                    v-bind="attrs"
                    v-on="on"
                    style="width:290px"
                  ></v-text-field>
                </template>
                <v-date-picker
                  v-model="date"

                  :active-picker.sync="activePicker"
                  min="1950-01-01"
                  @change="save"
                ></v-date-picker>
              </v-menu>
            </v-col>
            <v-col>
           <v-autocomplete
              v-model="usuarios.id"
              :items="usuarios"
              outlined
              label="Usuario"
              item-text="nome"
              item-value="id"
              :rules="rule"
              style="width:300px"
            >
            </v-autocomplete>
            </v-col>
          </v-row>
          <v-row>
            <v-col>
              <v-autocomplete
              v-model="livros.id"
              :items="livros"
              outlined
              multiple
              chips
              deletable-chips
              label="Livros"
              item-text="titulo"
              item-value="id"
              :rules="rule"
            >
            </v-autocomplete>
            </v-col>
          </v-row>
          <v-btn
          @click="cadastrar()">
            Cadastrar
          </v-btn>
           <v-btn
          to="/emprestimo/teste">
          teste
          </v-btn>
        </template>
       </v-form>
    </v-container>
  </v-container>
</template>

<script>
export default {
   name: 'CadastroEmprestimoPage',
  data () {
    return {
      activePicker: null,
      date: null,
      menu: false,
      usuarios: [],
      livros: [],
      valid: false,
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
  created () {
    this.getEmprestimo()
    this.getLivros()
  },
  methods: {
    async getEmprestimo (idEmprestimo) {
    try {
      let usuario = await this.$axios.$get(`http://localhost:3333/usuarios`)
      this.usuarios = usuario.data.usuario
    } catch (error) {
      this.$toast.error(`Ocorreu um erro no cadastro, contate o administrador`)
    }
    },
    async getLivros () {
      let livros = await this.$axios.$get(`http://localhost:3333/livros/disponiveis/nan`)
      this.livros = livros
    },
    async cadastrar () {
       if(!this.valid){
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }

        let emprestimo = {
          prazo: this.date,
          idUsuario: this.usuarios.id,
          devolucao: null,
          livros: this.livros.id
        };

        let response = await this.$axios.$post('http://localhost:3333/emprestimo', emprestimo);
        if(response.error){
          return this.$toast.error(response.message)
        }
        response = response
        this.$router.push('/empretimo')
        alert(`Emprestimo criado com sucesso, id ${response.id}.`)
        return this.$router.push('/emprestimo');

    },
    save (date) {
        this.$refs.menu.save(date)
    },
  
  }
}
</script>