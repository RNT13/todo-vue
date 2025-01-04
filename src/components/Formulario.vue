<script setup>
const props = defineProps(['cadastrarTarefa', 'tarefaTemp', 'trocarFiltro', 'editaTarefaTemp'])
</script>
<template>
    <form @submit.prevent="props.cadastrarTarefa">
        <div class="row">
            <div class="col-12 col-md-8 mb-2 mb-md-0">
                <input :value="props.tarefaTemp" @change="props.editaTarefaTemp" required
                    class="form-control" type="text" placeholder="Digite a descrição da tarefa">
            </div>
            <div class="col-12 col-md-2 mb-2 mb-md-0 d-flex justify-content-center">
                <button type="submit" class="btn btn-primary w-100 w-md-auto">Cadastrar</button>
            </div>
            <div class="col-12 col-md-2">
                <select @change="props.trocarFiltro" class="form-control">
                    <option value="todas">Todas as tarefas</option>
                    <option value="pendente">Pendentes</option>
                    <option value="finalizada">Finalizadas</option>
                    <option value="removida">Removidas</option>
                </select>
            </div>
        </div>
    </form>
</template>