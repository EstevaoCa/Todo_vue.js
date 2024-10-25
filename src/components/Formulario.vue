<script setup>
    const props = defineProps(['cadastrarTarefa', 'tarefaTemp', 'trocarFiltro', 'editarTarefaTemp'])
</script>

<template>
    <!-- Formulário para cadastrar uma nova tarefa -->
    <form @submit.prevent="props.cadastrarTarefa"> <!-- O evento de submit chama a função cadastrarTarefas -->
            <div class="row">
                <div class="col">
                    <!-- Campo de input para digitar a descrição da tarefa -->
                    <input 
                        :value="props.tarefaTemp" 
                        @change="props.editarTarefaTemp" 
                        type="text" 
                        placeholder="Digite a descrição da tarefa" 
                        class="form-control">
                </div>
                <div class="col-md-2">
                    <!-- Botão para cadastrar a tarefa -->
                    <button type="submit" class="btn btn-primary">Cadastrar</button>
                </div>
                <div class="col-md-2">
                    <!-- Select para escolher o filtro de tarefas: todas, pendentes ou finalizadas -->
                    <select @change="props.trocarFiltro" class="form-control">
                        <option value="todas">Todas Tarefas</option>
                        <option value="pendentes">Pendentes</option>
                        <option value="finalizadas">Finalizadas</option>
                    </select>
                </div>
            </div>
        </form>
</template>