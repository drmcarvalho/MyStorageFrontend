<template>
  <v-card class="mx-auto">
    <v-card-item>
      <v-btn color="primary" nuxt to="/cadastroprodutos">
        Cadastrar novo produto
      </v-btn>
    </v-card-item>
    <v-card-item>
      <v-card-title>
        Produtos
      </v-card-title>
      <v-card-subtitle>
        Lista de produtos
      </v-card-subtitle>
    </v-card-item>
    <v-card-item>
      <v-data-table :headers="headers" :items="produtos" item-key="productId"></v-data-table>
    </v-card-item>
  </v-card>
</template>

<script>
export default {
  name: 'Produtos',
  data: () => ({
    headers: [
      { text: 'Descrição', title: 'Descrição', value: 'productName', },
      { text: 'Preço', title: 'Preço', value: 'price', },
      { text: 'Saldo total em estoque', title: 'Saldo total em estoque', value: 'stockBalance', },
      { text: 'Estocado em', title: 'Estocado em', value: 'storages' }
    ],
    produtos: [],
  }),
  methods: {
    async obterProdutos() {
      try {
        const response = await this.$axios({
          method: "GET",
          url: '/Product/GetAll',
        })
        this.produtos = response.data
      } catch (error) {
        return alert(error)
      }
    }
  },
  mounted() {
    this.obterProdutos()
  }
}
</script>
