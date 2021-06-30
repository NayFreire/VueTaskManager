<template>
    <div :key="task.id" v-for="task in tasks">
        <Task 
            @toggle-reminder="$emit('toggle-reminder', task.id)" 
            @delete-task="$emit('delete-task', task.id)" 
            :task="task"
        />
    </div>
</template>

<script>
    import Task from './Task'

    export default{
        name: 'Tasks',
        components: {
            Task
        },
        props: {
            tasks: Array
        },
        // Since the Task component has the 'delete-task' emit, we need to use emits to not get the following warning: "Extraneous non-emits event listeners (deleteTask) were passed to component but could not be automatically inherited because component renders fragment or text root nodes. If the listener is intended to be a component custom event listener only, declare it using the "emits" option.""
        emits: ['delete-task', 'toggle-reminder']
    }
</script>
