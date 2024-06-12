<template>
  <v-card class="mx-auto">
    <v-card-item>
      <v-btn color="primary" nuxt to="/efetuarmovimentacao">
        Efetuar uma nova movimentação
      </v-btn>
    </v-card-item>
    <v-card-item>
      <v-card-title>
        Movimentações
      </v-card-title>
      <v-card-subtitle>
        Histórico
      </v-card-subtitle>
    </v-card-item>
    <v-card-item>
      <v-text-field label="Digite algo em seguida tecle enter para pesquisar" v-model="query" @keydown.enter="pesquisar" ></v-text-field>
      <v-data-table :headers="headers" :items="movimentacoes" item-key="movementId"></v-data-table>
    </v-card-item>
  </v-card>
</template>

<script>
export default {
  name: 'Movimentacoes',
  data: () => ({
    headers: [
      { text: 'Nome do produto', title: 'Nome do produto', value: 'productName', },
      { text: 'Estoque', title: 'Estoque', value: 'storageName', },
      { text: 'Quantidade movimentada', title: 'Quantidade movimentada', value: 'amount', },
      { text: 'Tipo de movimentação', title: 'Tipo de movimentação', value: 'type', },
      { text: 'Data e hora da movimentação', title: 'Data e hora da movimentação', value: 'movementDateFormatted', },
    ],
    movimentacoes: [],
    query: ''
  }),
  methods: {
    async obterHistoricoMovimentacoes() {
      try {
        const response = await this.$axios({
          method: "GET",
          url: '/StorageManager/Movement/GetAllHistoryMovements',
        })
        this.movimentacoes = response.data
      } catch (error) {
        return alert(error)
      }
    },
    async pesquisar(event) {
      event.preventDefault()
      if (this.query.length == 0) {
        this.obterHistoricoMovimentacoes()
        return
      }
      try {
        const response = await this.$axios({
          method: "GET",
          url: '/StorageManager/Movement/Query',
          params: {
            q: this.query
          }
        })

        this.movimentacoes = response.data
      } catch(error) {
        alert(error)
      }
    },
  },
  mounted() {
    this.obterHistoricoMovimentacoes()
  }
}
</script>
