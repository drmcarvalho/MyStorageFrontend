<template>
  <v-card class="mx-auto" max-width="800" hover>
    <v-card-item>
      <v-card-title>
        Editar
      </v-card-title>
      <v-card-subtitle>
        Estoque
      </v-card-subtitle>
    </v-card-item>
    <v-card-item>
      <v-sheet class="mx-auto" width="800">
        <v-form fast-fail @submit.prevent>
          <v-text-field v-model="model.identification" :rules="identificationRegras" label="Identificação do estoque">{{ model.identification }}</v-text-field>
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
      id: 0,
      identification: '',
    },
    identificationRegras: [
      value => {
        if (value?.length >= 3) return true

        return 'Identificação deve ter pelo menos 3 caracteres'
      },
    ],
  }),
  methods: {
    async submit() {
      try {
        await this.$axios({
          method: "PUT",
          url: '/StorageManager/Storage/Update',
          data: this.model,
        })

        this.$router.push({ name: 'estoques' })
      }
      catch (error) {
        console.log(error)
      }
    },
    async obterEstoque() {
      try {
        const config = {
          method: "GET",
          url: `/StorageManager/Storage/Get/${this.$route.params.id}`,
        }

        const estoque = await this.$axios(config)

        this.model.id = estoque.data.storageId
        this.model.identification = estoque.data.identification
      } catch (error) {
        console.log(error)
      }
    }
  },
  mounted() {
    this.obterEstoque()
  }
}
</script>
