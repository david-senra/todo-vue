<script setup>
import { reactive } from 'vue';
const estado = reactive ({
    erro: false,
    inputAtividade: "",
    filtro: 'todas',
    tarefaTemp: "",
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
    console.log(estado.tarefas);
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

const mostrarErro = () => (estado.erro == true) && (estado.tarefaTemp.length < 1);

const cadastrarTarefa = (evento) => {
    if (estado.tarefaTemp.length > 0) {
        const tarefaNova = {
        titulo: estado.tarefaTemp,
        finalizada: false
        };
        console.log(tarefaNova);
        estado.tarefas.push(tarefaNova);
        estado.tarefaTemp = "";
        estado.erro = false;
    }
    else {
        estado.erro = true;
    }
}

</script>

<!-- ------------------------------------------------------------------------------------------------------------------------------------------ -->

<template>
    <div class="container">
        <header class="header">
            <h1>Minhas Tarefas</h1>
            <p id="texto-erro">Você possui {{getTarefasPendentes().length}} tarefas pendentes!</p>
        </header>
        <form @submit.prevent="evento => cadastrarTarefa(evento)">
            <div class="cadastro">
                <div class="cadastro__secao" id="secao-atividade">
                    <input autocomplete="off" @keyup="evento => [(estado.erro = true), (estado.tarefaTemp = evento.target.value)]" @change="evento => estado.tarefaTemp = evento.target.value" id="texto-tarefa" type="text" placeholder="Descrição da tarefa:" :value="estado.tarefaTemp">
                    <p id="atividade-erro" class="atividade-erro" :class="{error: mostrarErro()}" style="margin: 0">A atividade precisa ser preenchida!</p>
                </div>
                <div class="cadastro__secao">
                    <button :disabled="estado.tarefaTemp.length < 1" class="cadastro__secao__button" type="submit">Cadastrar</button>
                </div>
                <div class="cadastro__secao">
                    <select @change="evento => estado.filtro = evento.target.value" class="select">
                        <option class="item" value="todas">Todas as Tarefas</option>
                        <option class="item" value="pendentes">Tarefas Pendentes</option>
                        <option class="item" value="finalizadas">Tarefas Finalizadas</option>
                    </select>
                </div>
            </div>
        </form>
        <ul class="tarefas">
            <li class="tarefas__item" v-for="tarefa in mostraTarefasFiltradas()">
                <input :checked="tarefa.finalizada" @change="evento => tarefa.finalizada = evento.target.checked" :id="tarefa.titulo" type="checkbox">
                <label :for="tarefa.titulo" :class="{done: tarefa.finalizada}">
                    {{tarefa.titulo}}
                </label>
            </li>
        </ul>
    </div>
</template>

<!-- ------------------------------------------------------------------------------------------------------------------------------------------- -->

<style lang="scss" scoped>
.container {
    max-width: 1024px;
    margin: 0 auto;
}
.header {
    width: 90%;
    padding: 5px;
    margin-bottom: 4px;
    margin-top: 4px;
    background-color: azure;
    border-radius: 10px;
    text-align: center;
}

.cadastro {
    width: 90%;
    display: flex;
    vertical-align: middle;
    text-align: center;
    align-items: center;
    justify-content: center;
    margin-top: 30px;

    &__secao {
        text-align: center;
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 10px;

        #texto-tarefa {
            margin-top: 0;
            margin-bottom: 0;
            width: 250px;
            height: 25px;
            border-radius: 5px;
            margin-right: 10px;
        }

        #texto-tarefa::placeholder {
            text-align: center;
        }

        &__button {
            background-color: rgb(82, 82, 247);
            color: #fff;
            border: transparent;
            border-radius: 10%;
            cursor: pointer;
            line-height: 30px;
            width: 70px;
            margin-right: 60px;

            &:hover {
                background-color: darken(rgb(82, 82, 247), 30%);
            }

            &:disabled {
                background-color: lighten(rgb(82, 82, 247), 25%);
                cursor: default;
            }
        }

        .select {
            width: 200px;
            padding: 0;
            font-size:13px;
            text-indent: calc(50% - 70px);
        }

        select dropdown {
            text-indent: calc(50% - 70px);
        }
    }
}

.tarefas {
    text-align: center;
    list-style-type: none;
    margin: 0;
    margin-top: 30px;
    padding: 0;
    width: 90%;

    &__item {
        font-size: 20px;
        margin-bottom: 5px;
        border-color: #ccc;
        label {
            margin-left: 10px;
        }
    }
}

.done {
    text-decoration: line-through;
}

#secao-atividade {
    display:inline;
    padding-top: 19px;
}

.atividade-erro {
    font-size: 13px;
    color: red;
    visibility: hidden;
}

.error {
    visibility: visible;
}

</style>
