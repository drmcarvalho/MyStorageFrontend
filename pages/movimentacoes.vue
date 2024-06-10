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
    ],
    movimentacoes: [],
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
    }
  },
  mounted() {
    this.obterHistoricoMovimentacoes()
  }
}
</script>
