<script setup>
import Cabecalho from './components/Cabecalho.vue';
import Formulario from './components/Formulario.vue';
import List from './components/List.vue';
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
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" />
    <Formulario :trocar-filtro="e => estado.filtro = e.target.value" :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="e => estado.tarefaTemp = e.target.value" :adcionar-tarefa="adcionarTarefa" />
    <List :tarefas="getTarefasFiltradas()"/>
  </div>
</template>

