<script setup>
    const props = defineProps(['tarefasPendentes'])
</script>

<template>
    <!-- Cabeçalho da página -->
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
        <h1>Minhas Tarefas</h1>
        <p>
            <!-- Mostra quantas tarefas pendentes o usuário tem -->
            Você possui {{ props.tarefasPendentes }} Tarefas pendentes
        </p>
    </header>
</template>