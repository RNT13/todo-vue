<script setup>
const props = defineProps(['tarefas', 'filtro', 'removerTarefa', 'readicionarTarefa']);
</script>
<template>
    <div>
        <p class="mt-5 text-center fs-3" v-if="props.tarefas.length === 0">
            <span v-if="props.filtro === 'finalizada'">Finalize suas tarefas</span>
            <span v-else-if="props.filtro === 'removida'">Não há tarefas removidas</span>
            <span v-else-if="props.filtro === 'todas'">Vai arrumar algo para fazer</span>
            <span v-else>Não há tarefas pendentes</span>
        </p>
        <ul v-else class="list-group mt-4">
            <li class="list-group-item d-flex justify-content-between align-items-center"
                v-for="tarefa in props.tarefas" :key="tarefa.titulo">
                <div>
                    <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada"
                        :id="tarefa.titulo" type="checkbox">
                    <label :class="{ done: tarefa.finalizada }" class="ms-3" :for="tarefa.titulo">
                        {{ tarefa.titulo }}
                    </label>
                </div>
                <button v-if="tarefa.finalizada || props.filtro === 'removida'"
                    @click="() => props.removerTarefa(tarefa)" class="btn btn-danger btn-sm">Remover</button>
                <button v-if="tarefa.removida" @click="() => props.readicionarTarefa(tarefa)"
                    class="btn btn-success btn-sm">Readicionar</button>
            </li>
        </ul>
    </div>
</template>

<style scoped>
    .done {
        text-decoration: line-through;
    }
</style>