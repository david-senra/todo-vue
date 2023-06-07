<script setup>
import { reactive } from 'vue';
import Cabecalho from './components/Cabecalho.vue';
import Formulario from './components/Formulario.vue';
import ListaTarefas from './components/ListaTarefas.vue';

const estado = reactive ({
    inputAtividade: "",
    filtro: 'todas',
    tarefaTemp: "",
    erro: false,
    tarefas: [
        {
            titulo: 'Varrer a Casa',
            finalizada: false
        },
        {
            titulo: 'Alimentar o cão',
            finalizada: false
        },
        {
            titulo: 'Comprar pão na padaria',
            finalizada: true
        }
    ],
})

const getTarefasPendentes = () => {
    console.log(estado);
    return estado.tarefas.filter(tarefa => !tarefa.finalizada);
}

const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada);
}

const mostraTarefasFiltradas = () => {
    const { filtro } = estado;
    switch (filtro) {
        case "pendentes":
            return getTarefasPendentes();
        case "finalizadas":
            return getTarefasFinalizadas();
        case "todas":
            return estado.tarefas;
    }
}

const trocarFiltro = (evento) => {
    estado.filtro = evento.target.value;
}

const editarTarefa = (evento) => {
    estado.tarefaTemp = evento.target.value
}

const cadastrarTaref = (evento) => {
    if (estado.tarefaTemp.length > 0) {
        const tarefaNova = {
        titulo: estado.tarefaTemp,
        finalizada: false
        };
        estado.tarefas.push(tarefaNova);
        estado.tarefaTemp = "";
        estado.erro = false;
    }
    else {
        estado.erro = true;
    }
}

const apagarErro = () => {
    console.log("teste");
    estado.erro = true;
}

const erroCadastro = () => estado.tarefaTemp.length == 0 && estado.erro == true;

const desabilitarBotao = () => estado.tarefaTemp.length == 0;

</script>

<!-- ------------------------------------------------------------------------------------------------------------------------------------------ -->

<template>
    <div class="container">
        <Cabecalho :tarefas-pendentes="getTarefasPendentes().length"/>
        <Formulario :trocar-filtro="trocarFiltro" 
                    :tarefa-temp="estado.tarefaTemp" 
                    :editar-taref="editarTarefa" 
                    :cadastra-tarefas="cadastrarTaref"
                    :apagar-erro="apagarErro"
                    :desabilitar-botao="desabilitarBotao()"
                    :marcar-erro="erroCadastro()" />
        <ListaTarefas :tarefas="mostraTarefasFiltradas()"/>
    </div>
</template>

<!-- ------------------------------------------------------------------------------------------------------------------------------------------- -->

<style lang="scss" scoped>
.container {
    max-width: 1024px;
    margin: 0 auto;
}
</style>
