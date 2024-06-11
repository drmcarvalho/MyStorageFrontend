<template>
  <v-card class="mx-auto" max-width="800" hover>
    <v-card-item>
      <v-card-title>
        Estoques
      </v-card-title>
      <v-card-subtitle>
        Cadastro de estoques
      </v-card-subtitle>
    </v-card-item>
    <v-card-item>
      <v-sheet class="mx-auto" width="800">
        <v-form fast-fail @submit.prevent>
          <v-text-field v-model="model.identification" :rules="identificationRegras" label="Identificação do estoque"></v-text-field>
          <v-btn class="mt-2" type="submit" @click="submit" block>Salvar</v-btn>
        </v-form>
      </v-sheet>
    </v-card-item>
  </v-card>
</template>

<script>
export default {
  name:'EstoqueCadastro',

  data: () => ({
    model: {
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
          method: "POST",
          url: '/StorageManager/Storage/Create',
          data: this.model,
        })

        this.redirect()
      }
      catch (error) { alert(error) }
    },
    redirect() {
      this.$router.push({ name: 'estoques' })
    }
  },
}
</script>

<style></style>
