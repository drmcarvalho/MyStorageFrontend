<template>
  <v-card class="mx-auto">
    <v-card-item>
      <v-btn color="primary" nuxt to="/cadastroestoques">
        Cadastrar novo estoque
      </v-btn>
    </v-card-item>
    <v-card-item>
      <v-card-title>
        Estoques
      </v-card-title>
      <v-card-subtitle>
        Lista de estoques
      </v-card-subtitle>
    </v-card-item>
    <v-card-item>
      <v-data-table :headers="headers" :items="estoques" item-key="storageId"></v-data-table>
    </v-card-item>
  </v-card>
</template>

<script>
export default {
  name:'Estoques',
  data: () => ({
    headers: [
      { text: 'Identificação do estoque', title: 'Identificação do estoque', value: 'identification', },
    ],
    estoques: []
  }),
  methods: {
    async obterEstoques() {
      try {
        const response = await this.$axios({
          method: "GET",
          url: '/StorageManager/Storage/GetAll',
        })
        this.estoques = response.data
      } catch (error) {
        return alert(error)
      }
    },
  },
  mounted() {
    this.obterEstoques()
  }
}
</script>
