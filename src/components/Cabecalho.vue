<script setup>
    const props = defineProps(['tarefasPendentes'])
</script>

<template>
    <header class="header">
        <h1>Minhas Tarefas</h1>
        <p id="texto-erro">Você possui {{props.tarefasPendentes}} tarefas pendentes!</p>
    </header>
</template>

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
</style>