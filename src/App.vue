<script setup>
import { reactive, ref } from 'vue';

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
  const {filtro} = estado;

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
  tarefa.removida = true;
}

const readicionarTarefa = (tarefa) => {
  tarefa.removida = false;
}

</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-4">
      <h1>Minhas tarefas</h1>
      <p>
        Você possui {{ getTarefasPendentes().length }} tarefas pendentes
      </p>
    </header>
    <form @submit.prevent="adicionarTarefa(novaTarefa)">
      <div class="row">
        <div class="col">
          <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" required  class="form-control" type="text" placeholder="Digite a descrição da tarefa">
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select @change="evento => estado.filtro = evento.target.value" class="form-control">
            <option value="todas">Todas as tarefas</option>
            <option value="pendente">Pendentes</option>
            <option value="finalizada">Finalizadas</option>
            <option value="removida">Removidas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item d-flex justify-content-between align-items-center" v-for="tarefa in getTarefasFiltradas()" :key="tarefa.id">
        <div>
          <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada"  :id="tarefa.titulo" type="checkbox" >
          <label :class="{ done: tarefa.finalizada }"  class="ms-3" :for="tarefa.titulo">
            {{ tarefa.titulo }}
          </label>
        </div>
        <button v-if="tarefa.finalizada" @click="removerTarefa(tarefa)" class="btn btn-danger btn-sm">Remover</button>
        <button v-if="tarefa.removida" @click="readicionarTarefa(tarefa)" class="btn btn-success btn-sm">Readicionar</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
  .done  {
    text-decoration: line-through;
  }
</style>
