<template>
  <v-card class="mx-auto" max-width="800" hover>
    <v-card-item>
      <v-card-title>
        Movimentação
      </v-card-title>
      <v-card-subtitle>
        Efetuar nova movimentação
      </v-card-subtitle>
    </v-card-item>
    <v-card-item>
      <v-sheet class="mx-auto" width="800">
        <v-form fast-fail @submit.prevent>
          <v-select v-model="model.productId" :items="produtosComboBox" item-text="productName" item-value="productId" label="Produto" :rules="produtoRegras" />
          <v-select v-model="model.storageId" :items="estoquesComboBox" item-text="identification" item-value="storageId" label="Estoque" :rules="estoqueRegras"/>
          <v-select v-model="model.type" :items="[{ key: 'Entrada', value: 'E' }, { key: 'Saida', value: 'S' }]" item-text="key" item-value="value" label="Tipo de movimentação E/S" :rules="tipoRegras" />
          <v-text-field v-model="model.amount" label="Quantidade" :rules="quantidadeRegras"></v-text-field>
          <v-btn class="mt-2" type="submit" @click="submit" block>Efetuar movimentação</v-btn>
        </v-form>
      </v-sheet>
    </v-card-item>
  </v-card>
</template>

<script>
export default {
  name: 'MovimentacaoCadastro',
  data: () => ({
    model: {
      productId: '',
      storageId: '',
      type: '',
      amount: ''
    },
    produtoRegras: [
      value => {
        if (!!value) return true

        return 'Selecione o produto'
      }
    ],
    estoqueRegras: [
      value => {
        if (!!value) return true

        return 'Selecione o estoque'
      }
    ],
    tipoRegras: [
      value => {
        if (!!value) return true

        return 'Selecione o tipo de movimentação'
      }
    ],
    quantidadeRegras: [
      value => {
        if (value > 0) return true

        return 'Quantidade deve ser acima de 0.'
      },
    ],
    produtosComboBox: [],
    estoquesComboBox: [],
  }),

  methods: {
    async submit() {

      try {
        await this.$axios({
          method: "POST",
          url: '/StorageManager/Movement/RegisterMovementInStorage',
          data: this.model,
        })

        this.redirect()
      }
      catch (error) {
        alert(error)
      }
    },
    async obterEstoques() {
      try {
        const response = await this.$axios({
          method: "GET",
          url: '/StorageManager/Storage/GetAll',
        })
        this.estoquesComboBox = response.data
      } catch (error) {
        return alert(error)
      }
    },
    async obterProdutos() {
      try {
        const response = await this.$axios({
          method: "GET",
          url: '/Product/GetAll',
        })
        this.produtosComboBox = response.data
      } catch (error) {
        return alert(error)
      }
    },
    redirect() {
      this.$router.push({ name: 'movimentacoes' })
    }
  },
  mounted() {
    this.obterEstoques()
    this.obterProdutos()
  }
}
</script>

<style></style>
