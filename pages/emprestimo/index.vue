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
        />
        </v-col>
        <v-col cols="2">
        </v-col>
        <v-col>
          <v-btn
           outlined
            color="blue"
            @click="cadastrar"
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
         item-key="id"
        :search="search"
      >
    <template v-slot:item.actions="{ item }">
        <v-icon
          small
          class="mr-2"
          color="yellow"
          @click="consulta(item)"
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
          v-if="!item.devolucao"
          small
          class="mr-2"
          outlined
          color="red"
          @click="encerrar(item)"
          style="margin-left: 30px"
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
    }
   
  },
  created () {
    this.getEmprestimo()
  },
  methods: {
    async getEmprestimo () {
      try {
        let emprestimos = await this.$axios.$get(`http://localhost:3333/emprestimo`)
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
          this.getEmprestimo()
        } 
      } catch (error) {
        this.$toast.error(`Ocorreu um erro ao deletar, contate o administrador`);
      }
     },
    async consulta (item) {
      try {
        this.$router.push({
          name: 'emprestimo-consulta',
          params: { id: item.id }
        });
      } catch (error) {
        this.$toast.error(`Ocorreu um erro ao consultar o emprestimo, contate o administrador`);
      }
    },
    async encerrar (item) {
     try {
        let emprestimo = {
          devolucao: new Date(Date.now()).toISOString().substring(0,10)
        }
        let response = await this.$axios.$post(`http://localhost:3333/emprestimo/${item.id }`, {devolucao: emprestimo.devolucao});
        this.getEmprestimo()
     } catch (error) {
      this.$toast.error(`Ocorreu um erro ao encerrar o emprestimo, contate o administrador`);
     }
    },
    async cadastrar () {
      try {
        this.$router.push({ name: 'emprestimo-cadastro' });
      } catch (error) {
         this.$toast.error(`Ocorreu um erro ao carregar a pagina de cadastro, contate o administrador`);
      }
    }
  }
}
</script>
