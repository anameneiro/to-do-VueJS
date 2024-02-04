<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>
        Você possui {{ getTarefasPendentes().length }} tarefas pendentes
      </p>
    </header>
    <form @submit.prevent="cadastraTarefa">
      <div class="row">
        <div class="col">
          <input v-model="estado.tarefaTemp" required type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select v-model="estado.filtro" class="form-control">
            <option value="todas">Todas tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="(tarefa, index) in getTarefasFiltradas()" :key="index">
        <input @change="() => tarefa.finalizada = !tarefa.finalizada" :checked="tarefa.finalizada" :id="'tarefa-' + index" type="checkbox">
        <label :class="{ done: tarefa.finalizada }" class="ms-3" :for="'tarefa-' + index">
          {{ tarefa.titulo }}
        </label>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { reactive } from 'vue';

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [
    { titulo: 'Estudar ES6', finalizada: false },
    { titulo: 'Estudar SASS', finalizada: false },
    { titulo: 'ir para a academia', finalizada: true }
  ]
});

const getTarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada);
};

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada);
};

const getTarefasFiltradas = () => {
  const { filtro } = estado;

  switch (filtro) {
    case 'pendentes':
      return getTarefasPendentes();
    case 'finalizadas':
      return getTarefasFinalizadas();
    default:
      return estado.tarefas;
  }
};

const cadastraTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false
  };
  estado.tarefas.push(tarefaNova);
  estado.tarefaTemp = '';
};
</script>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
