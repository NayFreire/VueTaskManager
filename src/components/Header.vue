<template>
    <header>
        <h1>{{title}}</h1>
        <Button 
            v-show="homePage"
            @btn-click="$emit('toggle-add-task')" 
            :text="showAddTask ? 'Close' : 'Add Task'" 
            :color="showAddTask ? 'red' : 'green'"
        />
    </header>
</template>

<script>
    import Button from './Button'

    export default {
        name: 'Header',
        props: {
            title: String,
            showAddTask: Boolean
        },
        components: {
            Button
        },
        computed: {
            homePage(){
                //If the path is '/', it means that we are in the homepage, so we need to show the Add Task button
                if(this.$route.path === '/'){
                    return true
                }
                //If we're not in the homepage, there's no need for the Add Task button to be shown
                else{
                    return false
                }
            }
        }
    }
</script>

<style scoped>
    header{
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>
