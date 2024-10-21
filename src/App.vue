<script setup>
// Importa a função `reactive` do Vue para criar um estado reativo
import { reactive } from 'vue';

// Criamos um objeto reativo que contém o estado da aplicação
const estado = reactive({
    filtro: 'todas',  // O filtro define quais tarefas serão exibidas: todas, pendentes ou finalizadas
    tarefaTemp: '',  // Variável temporária para armazenar a nova tarefa enquanto o usuário digita
    tarefas: [  // Lista de tarefas com seus títulos e status (finalizada ou não)
        {
            titulo: 'Estudar ES6',
            finalizada: false,
        },
        {
            titulo: 'Estudar SASS',
            finalizada: false,
        },
        {
            titulo: 'Ir para a academia',
            finalizada: true,
        },
    ]
})

// Função que retorna as tarefas pendentes (aquelas que não estão finalizadas)
const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada);
}

// Função que retorna as tarefas finalizadas
const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada);
}

// Função que retorna as tarefas com base no filtro selecionado
const getTarefasFiltradas = () => {
    const { filtro } = estado;  // Desestrutura o filtro do estado

    switch (filtro) {
        case 'pendentes':  // Se o filtro for "pendentes", retorna as tarefas pendentes
            return getTarefasPendentes();
        case 'finalizadas':  // Se o filtro for "finalizadas", retorna as tarefas finalizadas
            return getTarefasFinalizadas();
        default:  // Se o filtro for "todas", retorna todas as tarefas
            return estado.tarefas;
    }
}

// Função para cadastrar uma nova tarefa na lista de tarefas
const cadastrarTarefas = () => {
    const tarefaNova = {
        titulo: estado.tarefaTemp,  // Usa o valor temporário digitado pelo usuário como o título da nova tarefa
        finalizada: false,  // A tarefa nova começa como não finalizada
    }
    estado.tarefas.push(tarefaNova);  // Adiciona a nova tarefa à lista de tarefas
    estado.tarefaTemp = '';  // Limpa o campo temporário após cadastrar a tarefa
}
</script>

<template>
    <div class="container">
        <!-- Cabeçalho da página -->
        <header class="p-5 mb-4 mt-4-light rounded-3">
            <h1>Minhas Tarefas</h1>
            <p>
                <!-- Mostra quantas tarefas pendentes o usuário tem -->
                Você possui {{ getTarefasPendentes().length }} Tarefas pendentes
            </p>
        </header>

        <!-- Formulário para cadastrar uma nova tarefa -->
        <form @submit.prevent="cadastrarTarefas"> <!-- O evento de submit chama a função cadastrarTarefas -->
            <div class="row">
                <div class="col">
                    <!-- Campo de input para digitar a descrição da tarefa -->
                    <input 
                        :value="estado.tarefaTemp" 
                        @change="evento => estado.tarefaTemp = evento.target.value" 
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
                    <select @change="evento => estado.filtro = evento.target.value" class="form-control">
                        <option value="todas">Todas Tarefas</option>
                        <option value="pendentes">Pendentes</option>
                        <option value="finalizadas">Finalizadas</option>
                    </select>
                </div>
            </div>
        </form>

        <!-- Lista de tarefas -->
        <ul class="list-group mt-4">
            <!-- Itera sobre as tarefas filtradas de acordo com o filtro selecionado -->
            <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()" :key="tarefa.titulo">
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
    </div>
</template>

<style scoped>
/* Estilo para riscar a tarefa finalizada */
.done {
    text-decoration: line-through;
}
</style>
