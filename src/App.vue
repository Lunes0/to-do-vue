<script setup>
import { reactive } from 'vue';

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [
    { descricao: 'Estudar Vue.js', finalizada: false },
    { descricao: 'Estudar SASS', finalizada: false },
    { descricao: 'Ir no mercado', finalizada: true },
  ]
})

const getTarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada);
}

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada);
}

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
}

const adcionarTarefa = () => {
  const tarefaNova = {
    descricao: estado.tarefaTemp,
    finalizada: false,
  }
  estado.tarefas.push(tarefaNova);
  estado.tarefaTemp = '';
}

</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>
        Você possui {{ getTarefasPendentes().length }} tarefas pendentes.
      </p>
    </header>
    <form @submit.prevent="adcionarTarefa">
      <div class="row">
        <div class="col">
          <input :value="estado.tarefaTemp" @change="e => estado.tarefaTemp = e.target.value" required type="text" placeholder="Digite uma tarefa" class="form-control" />
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Adicionar</button>
        </div>
        <div class="col-md-2">
          <select @change="e => estado.filtro = e.target.value" class="form-control">
            <option value="todas">Todas as tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
        <input @change="e => tarefa.finalizada = e.target.checked" :checked="tarefa.finalizada" :id="tarefa.descricao" type="checkbox">
        <label :class="{ done: tarefa.finalizada }" class="ms-3" :for="tarefa.descricao">
          {{ tarefa.descricao }}
        </label>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.done {
  text-decoration: line-through;
  color: #999;
}
</style>
