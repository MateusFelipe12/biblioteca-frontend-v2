<template>
  <v-container>
    <h1 style="color:aquamarine ;">Consulta dos emprestimos</h1>
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
        <v-col cols="2">
          <v-btn
            outlined
            color="grey"
            @click="getEmprestimo(search)"
          >
            Pesquisar
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
           outlined
          color="blue"
         
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="emprestimo"
      >
    <template v-slot:item.actions="{ item }">
        <v-icon
          small
          class="mr-2"
          color="yellow"
          @click="update(item)"
        >
          mdi-magnify
        </v-icon>
        <v-icon
          small
          color="red"
          @click="deletar(item)"
        >
          mdi-delete
        </v-icon>
        <v-btn
          small
          class="mr-2"
          outlined
          color="red"
          @click="encerrar(item)"
        >
          Encerrar
        </v-btn>
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
      emprestimo: [],
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
  },
  methods: {
    async getEmprestimo (idEmprestimo) {
    try {
      let emprestimos = await this.$axios.$get(`http://localhost:3333/emprestimo`)
      console.log(emprestimos[0]);
      this.emprestimo = emprestimos
    } catch (error) {
      this.$toast.error(`Ocorreu um erro no cadastro, contate o administrador`)
    }
    },
     async deletar (item) {
      try {
        if (confirm(`Deseja deletar o emprestimo id ${item.id}?`)) {
          let response = await this.$axios.$post('http://localhost:3333/emprestimo/deletar', { id: item.id });
          this.$toast(response.message)
        } 
      } catch (error) {
        this.$toast.error(`Ocorreu um erro ao deletar, contate o administrador`);
      }
     },
    async update (item) {
      this.$router.push({
        name: 'emprestimo-update',
        params: { id: item.id }
      });
    },
    async encerrar (item) {
      let emprestimo = {
        devolucao: new Date(Date.now()).toISOString().substring(0,10)
      }
      let response = await this.$axios.$post(`http://localhost:3333/emprestimo/${item.id }`, {devolucao: emprestimo.devolucao});
      this.getEmprestimo()
    }
  }
}
</script>
