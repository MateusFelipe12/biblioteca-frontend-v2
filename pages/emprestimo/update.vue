<template>
  <v-container>
    <h1>Empretimo id {{emprestimos.id}}</h1>
    <v-template>
      <v-row>
        <v-col>
          <v-text-field
            label="Emprestimo"
            v-model="emprestimos.id"
            outlined
            disabled
            style="width:150px"
          />
        </v-col>
        <v-col>
           <v-text-field
            label="Prazo para devolução"
            v-model="emprestimos.prazo"
            outlined
            disabled
            style="width:200px"
          />
        </v-col>
         <v-col>
           <v-text-field
            label="Devolução"
            v-model="emprestimos.devolucao"
            outlined
            disabled
            style="width:200px"
          />
        </v-col>
        <v-col>
          <v-text-field
            label="id Usuario"
            v-model="emprestimos.idUsuario"
            outlined
            disabled
            style="width:150px"
          />
        </v-col>
      </v-row>
      <v-row>
      <v-container>
        <v-data-table
          :headers="headers"
          :items="emprestimos.livros"
        >
          </v-data-table>
        </v-container>
      </v-row>
    </v-template>
  </v-container>
</template>

<script>
export default {
 name: 'UpdateEmprestimosPage',
  data () {
    return {
      emprestimos: [],
      calendario: false,
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
        { text: "", value: "actions" },
      ]
    }
  },
created () {
  this.getEmprestimo (this.$route.params.id)
},
methods:{
  async getEmprestimo (idEmprestimo) {
    try {
      this.emprestimos = await this.$axios.$get(`http://localhost:3333/emprestimo/${idEmprestimo}`)
      this.emprestimos.devolucao  = this.emprestimos.devolucao ? this.emprestimos.devolucao : "Emprestado"
      console.log(this.emprestimos);
    } catch (error) {
      this.$toast.error(`Ocorreu um erro no cadastro, contate o administrador`)
    }
  },
}
}
</script>