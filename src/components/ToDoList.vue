<template>
    <!-- Ingreso de tareas -->
    <div class="bg-gray-100 min-h-screen flex flex-col items-center py-8">
        <div class="w-4/5 md:w-2/5 bg-white p-6 rounded-lg shadow-2xl">
            <h1 class="text-4xl font-bold text-center text-rose-600 mb-4">TO DO List</h1>
            <form @submit.prevent="addTask" class="flex flex-col gap-4">
                <input v-model="newTodo" type="text" placeholder="What's your next task?"
                    class="p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-violet-400" />
            </form>

            <!-- Listado tareas -->
            <ul class="mt-4 space-y-3">
                <li v-for="(task, index) in filteredTasks" :key="index" :class="[
                    'flex items-center p-3 rounded-lg shadow',
                    task.completed ? 'bg-green-100' : 'bg-red-100'
                ]">
                    <input type="checkbox" v-model="task.completed"
                        class="mr-3 w-5 h-5 appearance-none forced-colors:appearance-auto p-3 rounded-full border-2 border-gray-300 checked:bg-green-300 checked:border-green-500 cursor-pointer" />
                    <span :class="{ 'line-through text-gray-400': task.completed }" class="text-gray-700">
                        {{ task.text }}
                    </span>
                    <button @click="deleteTask(index)"
                        class="ml-auto px-2 py-1 rounded-lg border-2 border-gray-500/50 text-slate-400 hover:text-gray-900">X</button>
                </li>
            </ul>

            <!-- Botones de filtro -->
            <div v-if="tasks.length > 0" class="mt-6 flex justify-around">
                <button v-for="(filterBtn, index) in filters" :key="index" @click="filter = filterBtn.value" :class="[
                    'px-4 py-2 rounded-lg shadow text-white',
                    filter === filterBtn.value ? 'ring-2 ring-offset-2' : '',
                    filterBtn.value === 'all' ? 'bg-blue-500 hover:bg-blue-600' :
                        filterBtn.value === 'completed' ? 'bg-green-500 hover:bg-green-600' :
                            'bg-red-500 hover:bg-red-600'
                ]">
                    {{ filterBtn.label }}
                </button>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTodo = ref('')
const tasks = ref([])
const filter = ref('all')

//Filtros
const filters = [
    { label: 'All', value: 'all' },
    { label: 'Completed', value: 'completed' },
    { label: 'Pending', value: 'pending' }
]

// Filtrar tareas 
const filteredTasks = computed(() => {
    if (filter.value === 'completed') {
        return tasks.value.filter(task => task.completed)
    } else if (filter.value === 'pending') {
        return tasks.value.filter(task => !task.completed)
    }
    return tasks.value
})

// Agregar nueva tarea
const addTask = () => {
    if (newTodo.value.trim()) {
        tasks.value.push({ text: newTodo.value.trim(), completed: false })
        newTodo.value = ''
    }
}

// Eliminar tarea
const deleteTask = (index) => {
    tasks.value.splice(index, 1)
}
</script>