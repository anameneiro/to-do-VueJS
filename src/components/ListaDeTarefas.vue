<script setup>
    const props = defineProps(['tarefas']);
</script>

<template>
    <ul class="list-group mt-4">
        <li class="list-group-item" v-for="(tarefa) in props.tarefas">
        <input @change="() => tarefa.finalizada = !tarefa.finalizada" :checked="tarefa.finalizada" :id="'tarefa-' + index" type="checkbox">
        <label :class="{ done: tarefa.finalizada }" class="ms-3" :for="'tarefa-' + index">
            {{ tarefa.titulo }}
        </label>
        </li>
    </ul>
</template>