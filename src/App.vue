<script setup>
import { reactive, ref } from 'vue';
import Cabecalho from './components/Cabecalho.vue';
import Formulario from './components/Formulario.vue';
import ListaDeTarefas from './components/ListaDeTarefas.vue';

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [
    {
      titulo: 'estudar ES6',
      finalizada: false,
      removida: false,
    },
    {
      titulo: 'estudar SASS',
      finalizada: false,
      removida: false,
    },
    {
      titulo: 'ir para a academia',
      finalizada: true,
      removida: false,
    }
  ]
})

const getTarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada && !tarefa.removida)
}

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada && !tarefa.removida)
}

const getTarefasRemovidas = () => {
  return estado.tarefas.filter(tarefa => tarefa.removida)
}

const getTarefasFiltradas = () => {
  const { filtro } = estado;

  switch (filtro) {
    case 'pendente':
      return getTarefasPendentes();
    case 'finalizada':
      return getTarefasFinalizadas();
    case 'removida':
      return getTarefasRemovidas();
    default:
      return estado.tarefas.filter(tarefa => !tarefa.removida);
  }
}

const adicionarTarefa = (titulo) => {
  const novaTarefa = {
    titulo: estado.tarefaTemp,
    finalizada: false,
    removida: false,
  }
  estado.tarefas.push(novaTarefa);
  estado.tarefaTemp = '';
}

const removerTarefa = (tarefa) => {
  if (estado.filtro === 'removida') {
    const index = estado.tarefas.indexOf(tarefa);
    if (index > -1) {
      estado.tarefas.splice(index, 1);
    }
  } else {
    tarefa.removida = true;
  }
}

const readicionarTarefa = (tarefa) => {
  tarefa.removida = false;
}

</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" />
    <Formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp"
      :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastrar-tarefa="adicionarTarefa" />
    <ListaDeTarefas :tarefas="getTarefasFiltradas()" :filtro="estado.filtro" :remover-tarefa="removerTarefa"
      :readicionar-tarefa="readicionarTarefa" />
  </div>
</template>