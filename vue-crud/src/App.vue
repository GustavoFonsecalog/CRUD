<template>
  <div>
    <v-app>
      <v-container>
        <h1 class="text-center mb-4">To-Do List</h1>

        <!-- Formulário de cadastro de tarefa -->
        <v-form @submit.prevent="adicionarTarefa">
          <v-row align="center">
            <v-col cols="12" sm="6" md="4">
              <v-text-field
                v-model="novaTarefa.descricao"
                label="Tarefa"
                outlined
                required
              ></v-text-field>
            </v-col>
            <v-col cols="12" sm="6" md="4">
              <v-text-field
                v-model="novaTarefa.prazo"
                label="Prazo"
                outlined
                type="date"
              ></v-text-field>
            </v-col>
            <v-col cols="12" sm="6" md="4">
              <v-text-field
                v-model="novaTarefa.responsavel"
                label="Responsável"
                outlined
              ></v-text-field>
            </v-col>
          </v-row>
          <v-btn type="submit" color="primary" class="mb-8">Adicionar</v-btn>
        </v-form>

        <!-- Listagem de tarefas -->
        <v-row>
          <v-col
            cols="12"
            sm="6"
            md="4"
            v-for="tarefa in tarefas"
            :key="tarefa.id"
          >
            <v-card>
              <v-card-text>
                <v-checkbox
                  v-model="tarefa.concluida"
                  @change="atualizarTarefa(tarefa)"
                ></v-checkbox>
                <span :class="{ concluida: tarefa.concluida }">{{
                  tarefa.descricao
                }}</span>
                <div v-if="tarefa.prazo">Prazo: {{ tarefa.prazo }}</div>
                <div v-if="tarefa.responsavel">
                  Responsável: {{ tarefa.responsavel }}
                </div>
              </v-card-text>
              <v-card-actions>
                <v-btn @click="editarTarefa(tarefa)" color="primary"
                  >Editar</v-btn
                >
                <v-btn @click="excluirTarefa(tarefa)" color="error"
                  >Excluir</v-btn
                >
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-app>
  </div>
</template>

<script>
export default {
  data() {
    return {
      novaTarefa: {
        descricao: "",
        prazo: "",
        responsavel: "",
      },
      tarefas: [],
    };
  },
  mounted() {
    // Recupera as tarefas do localStorage ao carregar o componente
    const tarefas = JSON.parse(localStorage.getItem("tarefas"));
    if (tarefas) {
      this.tarefas = tarefas;
    }
  },
  watch: {
    // Salva as tarefas no localStorage sempre que houver uma alteração
    tarefas: {
      handler() {
        localStorage.setItem("tarefas", JSON.stringify(this.tarefas));
      },
      deep: true,
    },
  },
  methods: {
    adicionarTarefa() {
      if (this.novaTarefa.descricao) {
        const novaTarefa = {
          id: Date.now(),
          descricao: this.novaTarefa.descricao,
          prazo: this.novaTarefa.prazo,
          responsavel: this.novaTarefa.responsavel,
          concluida: false,
        };
        this.tarefas.push(novaTarefa);
        this.novaTarefa.descricao = "";
        this.novaTarefa.prazo = "";
        this.novaTarefa.responsavel = "";
      }
    },
    atualizarTarefa(tarefa) {
      // Atualiza a tarefa
      // Aqui você pode fazer uma requisição para o backend e atualizar no servidor

      console.log("Tarefa atualizada:", tarefa);
    },
    editarTarefa(tarefa) {
      const novaDescricao = prompt(
        "Digite a nova descrição:",
        tarefa.descricao
      );
      if (novaDescricao) {
        tarefa.descricao = novaDescricao;
        console.log("Tarefa editada:", tarefa);
      }
    },
    excluirTarefa(tarefa) {
      const confirmacao = confirm("Tem certeza que deseja excluir a tarefa?");
      if (confirmacao) {
        const index = this.tarefas.findIndex((item) => item.id === tarefa.id);
        this.tarefas.splice(index, 1);
        console.log("Tarefa excluída:", tarefa);
      }
    },
  },
};
</script>

<style>
.concluida {
  text-decoration: line-through;
}
</style>
