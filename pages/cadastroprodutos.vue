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
  name:'ProdutoCadastro',

  data: () => ({
    model: {
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
          method: "POST",
          url: '/Product/Create',
          data: this.model,
        })

        this.model.name = ''
        this.model.price = ''
      }
      catch (error) { alert(error) }
    },
  },
}
</script>

<style></style>
