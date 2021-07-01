<template>
	<AddTask v-show="showAddTask" @add-task="addTask"/>
	<Tasks 
		@toggle-reminder="toggleReminder" 
		@delete-task="deleteTask" 
		:tasks="tasks"
	/>
</template>

<script>
    import Tasks from '../components/Tasks'
    import AddTask from '../components/AddTask'

    export default{
        name: 'Home',
        props: {
            showAddTask: Boolean
        },
        components:{
            Tasks,
            AddTask
        },
        data(){
            return{
                tasks: []
            }
        },
        methods:{
			async deleteTask(id){
				if(confirm("Are you sure?")){
					const res = await fetch(`api/tasks/${id}`, {
						method: 'DELETE',
					})

					res.status === 200 ? (this.tasks = this.tasks.filter((task) => task.id !== id)) : alert('Error deleting task')

					
				}
			},
			async toggleReminder(id){
				const taskToToggle = await this.fetchTask(id)
				const updTask = {...taskToToggle, reminder: !taskToToggle.reminder}

				const res = await fetch(`api/tasks/${id}`, {
					method: 'PUT',
					headers: {
						'Content-type': 'application/json'
					},
					body: JSON.stringify(updTask)
				})

				const data = await res.json()

				this.tasks = this.tasks.map(
					//If task.id === id, the task reminder will change it's value. 
					(task) => task.id === id ? 
					{...task, reminder: data.reminder} : task
				)
			},
			async addTask(newTask){
				console.log('new', newTask)
				const res = await fetch('api/tasks', {
					method:'POST',
					headers:{
						'Content-type': 'application/json'
					},
					body: JSON.stringify(newTask)
				})
				const data = await res.json()
				
				this.tasks = [...this.tasks, data]
			},
			async fetchTasks(){
				//Getting the data from the file db.json
				const res = await fetch('api/tasks')
				
				const data = await res.json()
				console.log('data', data)

				return data
			},
			async fetchTask(id){
				//Getting the data from the file db.json
				const res = await fetch(`api/tasks/${id}`)

				const data = await res.json()
				return data
			}
		},		
		async created(){ 
			//Attribuing to tasks the data from the json file
			this.tasks = await this.fetchTasks()
		}
    }
</script>
