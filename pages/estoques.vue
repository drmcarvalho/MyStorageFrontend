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
      <v-text-field label="Digite algo em seguida tecle enter para pesquisar" v-model="query" @keydown.enter="pesquisar" ></v-text-field>
      <v-data-table :headers="headers" :items="estoques" item-key="storageId">
        <template v-slot:item.controls="props">
          <v-btn class="mx-2" fab dark small color="blue" @click="editar(props)">
            <v-icon dark>mdi-pencil</v-icon>
          </v-btn>
        </template>
      </v-data-table>
    </v-card-item>
  </v-card>
</template>

<script>
export default {
  name: 'Estoques',
  data: () => ({
    headers: [
      { text: 'Identificação do estoque', title: 'Identificação do estoque', value: 'identification' },
      { text: "", value: "controls", sortable: false }
    ],
    estoques: [],
    query: "",
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
    editar(row) {
      this.$router.push({ path: `estoque/${row.item.storageId}` })
    },
    async pesquisar(event) {
      event.preventDefault()
      if (this.query.length == 0) {
        this.obterEstoques()
        return
      }
      try {
        const response = await this.$axios({
          method: "GET",
          url: '/StorageManager/Storage/Query',
          params: {
            q: this.query
          }
        })

        this.estoques = response.data
      } catch(error) {
        alert(error)
      }
    }
  },
  mounted() {
    this.obterEstoques()
  },
}
</script>
