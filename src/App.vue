<script setup>
    // Importa a função `reactive` do Vue para criar um estado reativo
    import { reactive } from 'vue';
    import Cabecalho from './components/Cabecalho.vue';
    import Formulario from './components/Formulario.vue';
    import ListaDeTarefas from './components/ListaDeTarefas.vue';

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
        <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" />
        <Formulario 
        :tarefa-temp="estado.tarefaTemp" 
        :editar-tarefa-temp="evento => estado.tarefaTemp = evento.targe.value"
        :cadastrar-tarefa="cadastrarTarefas"
        :trocar-filtro="evento => estado.filtro = evento.target.value"
        />
        <ListaDeTarefas :tarefas="getTarefasFiltradas()"
        />
    </div>
</template>


