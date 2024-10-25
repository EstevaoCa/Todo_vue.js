<script setup>
    const props = defineProps(['tarefas',])
</script>
<template>
    <!-- Lista de tarefas -->
    <ul class="list-group mt-4">
        <p v-if="tarefas.length === 0">
            Nenhuma tarefa {{ filtro === 'pendentes' ? 'pendente' : 'finalizada' }}.
        </p>
        <!-- Itera sobre as tarefas filtradas de acordo com o filtro selecionado -->
        <li class="list-group-item" v-for="tarefa in props.tarefas" :key="tarefa.titulo">
            <!-- Checkbox para marcar a tarefa como finalizada -->
            <input 
                @change="evento => tarefa.finalizada = evento.target.checked" 
                :checked="tarefa.finalizada" 
                :id="tarefa.titulo" 
                type="checkbox">
            <!-- Rótulo da tarefa com uma classe condicional que risca a tarefa se ela estiver finalizada -->
            <label :class="{ done: tarefa.finalizada }" class="ms-3" :for="tarefa.titulo">
                {{ tarefa.titulo }} <!-- Exibe o título da tarefa -->
            </label>
        </li>
    </ul>
</template>
<style scoped>
    /* Estilo para riscar a tarefa finalizada */
    .done {
        text-decoration: line-through;
    }
</style>

Aparecer a mensagem quando tarefas pendentes tiver vazio 