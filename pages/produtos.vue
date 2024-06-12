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
      <v-text-field label="Digite algo em seguida tecle enter para pesquisar" v-model="query" @keydown.enter="pesquisar" ></v-text-field>
      <v-data-table :headers="headers" :items="produtos" item-key="productId">
        <template v-slot:item.edit="props">
          <v-btn class="mx-2" fab dark small color="blue" @click="editar(props)">
            <v-icon dark>mdi-pencil</v-icon>
          </v-btn>
        </template>
        <template v-slot:item.delete="props">
          <v-btn class="mx-2" fab dark small color="red" @click="deletar(props)">
            <v-icon dark>mdi-delete</v-icon>
          </v-btn>
        </template>
      </v-data-table>
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
      { text: 'Estocado em', title: 'Estocado em', value: 'storages' },
      { text: "", value: "delete", sortable: false },
      { text: "", value: "edit", sortable: false }
    ],
    produtos: [],
    query: '',
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
    },
    async deletarProduto(productId) {
      try {
        await this.$axios({
          method: 'DELETE',
          url: `/Product/Delete/${productId}`
        })

        this.obterProdutos()
      } catch (error) {
        alert(error)
      }
    },
    async deletar(row) {
      this.deletarProduto(row.item.productId)
    },
    editar(row) {
      this.$router.push({ path: `produto/${row.item.productId}` })
    },
    async pesquisar(event) {
      event.preventDefault()
      if (this.query.length == 0) {
        this.obterProdutos()
        return
      }
      try {
        const response = await this.$axios({
          method: "GET",
          url: '/Product/Query',
          params: {
            q: this.query
          }
        })

        this.produtos = response.data
      } catch(error) {
        alert(error)
      }
    },
  },
  mounted() {
    this.obterProdutos()
  }
}
</script>
