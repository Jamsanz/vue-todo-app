<template>
    <ul :key="task.id" v-for="task in tasks">
        <Task @toggle-reminder="$emit('toggle-reminder', task.id)" @delete-task="$emit('delete-task', task.id)" :task="task" />
    </ul>
</template>

<script>
import Task from './Task.vue';
export default {
    name: 'Tasks',
    props:{
        tasks: Array
    },
    components:{
        Task
    },
    emits: ['delete-task', 'toggle-reminder']
}
</script>