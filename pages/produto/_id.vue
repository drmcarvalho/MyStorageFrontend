<template>
  <v-card class="mx-auto" max-width="800" hover>
    <v-card-item>
      <v-card-title>
        Produtos
      </v-card-title>
      <v-card-subtitle>
        Cadastro de produtos
      </v-card-subtitle>
    </v-card-item>
    <v-card-item>
      <v-sheet class="mx-auto" width="800">
        <v-form fast-fail @submit.prevent>
          <v-text-field v-model="model.name" :rules="nameRegras" label="Nome do produto"></v-text-field>
          <v-text-field v-model="model.price" :rules="precoRegras" label="Preço" prefix="R$"></v-text-field>
          <v-btn class="mt-2" type="submit" @click="submit" block>Salvar</v-btn>
        </v-form>
      </v-sheet>
    </v-card-item>
  </v-card>
</template>

<script>
export default {

  data: () => ({
    model: {
      productId: 0,
      name: '',
      price: '',
    },
    nameRegras: [
      value => {
        if (value?.length > 3) return true

        return 'Nome deve ter pelo menos 3 caracteres'
      },
    ],
    precoRegras: [
      value => {
        if (value > 0) return true

        return 'Preço deve ser acima de 0.'
      },
    ],
  }),

  methods: {
    async submit() {
      try {
        await this.$axios({
          method: "PUT",
          url: '/Product/Update',
          data: this.model,
        })

        this.redirect()
      } catch (error) {
        alert(error)
      }
    },
    async obterProduto() {
      try {
        const config = {
          method: "GET",
          url: `/Product/Get/${this.$route.params.id}`,
        }
        const produto = await this.$axios(config)

        this.model.productId = produto.data.productId
        this.model.name = produto.data.productName
        this.model.price = produto.data.price
      } catch (error) {
        alert(error)
      }
    },
    redirect() {
      this.$router.push({ name: 'produtos' })
    }
  },
  mounted() {
    this.obterProduto()
  }
}
</script>

<style></style>
