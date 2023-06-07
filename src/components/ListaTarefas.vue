<script setup>
    const props = defineProps (['tarefas'])
</script>

<template>
    <ul class="tarefas">
        <li class="tarefas__item" v-for="tarefa in props.tarefas">
            <input :checked="tarefa.finalizada" @change="evento => tarefa.finalizada = evento.target.checked" :id="tarefa.titulo" type="checkbox">
            <label :for="tarefa.titulo" :class="{done: tarefa.finalizada}">
                {{tarefa.titulo}}
            </label>
        </li>
    </ul>
</template>

<style lang="scss" scoped>
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
</style>