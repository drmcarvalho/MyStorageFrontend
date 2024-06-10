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
          <v-select v-model="model.productId" :items="produtosComboBox" item-text="productName" item-value="productId" label="Produto" />
          <v-select v-model="model.storageId" :items="estoquesComboBox" item-text="identification" item-value="storageId" label="Estoque" />
          <v-select v-model="model.type" :items="[{ key: 'Entrada', value: 'E' }, { key: 'Saida', value: 'S' }]" item-text="key" item-value="value" label="Tipo de movimentação E/S" />
          <v-text-field v-model="model.amount" label="Quantidade"></v-text-field>
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

        this.model.productId = ''
        this.model.storageId = ''
        this.model.type = ''
        this.model.amount = ''
      }
      catch (error) { alert(error) }
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
    }
  },
  mounted() {
    this.obterEstoques()
    this.obterProdutos()
  }
}
</script>

<style></style>
