<template>
  <v-container class="pa-4 text-center">
    <v-row>
      <v-col>
        <h1 style="color: white">Bem-vindo</h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <h1 style="color: white">Loja</h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-btn style="background-color: green; color: white;" rounded="xl" to="/"> Ir tela usuário </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
definePageMeta({
  layout: "admin",
});
export default {
  name: "Index",
  layout: "admin",
  data: () => {
    return {
      ativo: false,
      dialog: false,
      valor: 0,
    };
  },

  watch: {
    dialog(evento) {
      this.valor++;
    },
  },

  methods: {
    async getItens() {
      const response = await this.$api.get("/products");
      this.items = response.data;
      this.loading = false;
    },
    async deleteItem(items) {
      if (confirm(`Deseja deletar o registro com id ${items.id}`)) {
        const response = await this.$api.post("/products/destroy", {
          id: items.id,
        });
        if (response.type == "error") {
          alert(response.message);
        }
      }
      await this.getItens();
    },
    async persist() {
      if (this.atividade.id) {
        const response = await this.$api.post(
          `/products/persist/${this.atividade.id}`,
          this.atividade
        );
      } else {
        const response = await this.$api.post(
          "/products/persist",
          this.atividade
        );
      }
      this.resetAtividade();
      await this.getItems();
    },
    abrirDialog() {
      this.dialog = true;
      this.valor++;
    },
  },
};
</script>
