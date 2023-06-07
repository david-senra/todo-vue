<script setup>
    const props = defineProps (['cadastraTarefas', 'tarefaTemp', 'trocarFiltro', 'apagarErro', 'marcarErro', 'desabilitarBotao', 'editarTaref'])
</script>

<template>
    <form @submit.prevent="evento => props.cadastraTarefas(evento)">
        <div class="cadastro">
            <div class="cadastro__secao" id="secao-atividade">
                <input autocomplete="off" @keyup="evento => [props.editarTaref(evento), props.apagarErro()]" @change="evento => props.editarTaref(evento)" id="texto-tarefa" type="text" placeholder="Descrição da tarefa:" :value="props.tarefaTemp">
                <p id="atividade-erro" class="atividade-erro" :class="{ error: props.marcarErro }" style="margin: 0">A atividade precisa ser preenchida!</p>
            </div>
            <div class="cadastro__secao">
                <button :disabled="props.desabilitarBotao" class="cadastro__secao__button" type="submit">Cadastrar</button>
            </div>
            <div class="cadastro__secao">
                <select @change="evento => props.trocarFiltro(evento)" class="select">
                    <option class="item" value="todas">Todas as Tarefas</option>
                    <option class="item" value="pendentes">Tarefas Pendentes</option>
                    <option class="item" value="finalizadas">Tarefas Finalizadas</option>
                </select>
            </div>
        </div>
    </form>
</template>

<style lang="scss" scoped>
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
            margin-top: 15px;
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

.atividade-erro {
    font-size: 13px;
    color: red;
    visibility: hidden;
}

.error {
    visibility: visible;
}

#secao-atividade {
    display: block;
}

</style>