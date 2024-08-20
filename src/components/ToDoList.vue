<template>
    <div class="todo-list">
        <h1>To Do List</h1>
        <div class="input">
            <input v-model="newTask" @keyup.enter="addOrUpdateTask" placeholder="Agregar o actualizar una tarea" />
            <button @click="addOrUpdateTask">
                {{ isEditing ? 'Actualizar' : 'Agregar' }}
            </button>
        </div>
        <ul>
            <li v-for="(task, index) in tasks" :key="index">
                <label>
                    <input type="checkbox" v-model="task.completed" />
                </label>
                <br />
                <span :class="{ completed: task.completed }">
                    {{ task.text }}
                </span>
                <div class="actions">
                    <button class="edit" @click="editTask(index)">Editar</button>
                    <button class="delete" @click="deleteTask(index)">Eliminar</button>
                </div>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'ToDoList',
    data() {
        return {
            newTask: '',
            tasks: [],
            isEditing: false,
            currentTaskIndex: null
        }
    },
    methods: {
        addOrUpdateTask() {
            if (this.newTask.trim()) {
                if (this.isEditing) {
                    this.tasks[this.currentTaskIndex].text = this.newTask
                    this.isEditing = false
                } else {
                    this.tasks.push({ text: this.newTask, completed: false })
                }
                this.newTask = ''
                this.currentTaskIndex = null
            }
        },
        editTask(index) {
            this.newTask = this.tasks[index].text
            this.isEditing = true
            this.currentTaskIndex = index
        },
        deleteTask(index) {
            this.tasks.splice(index, 1)
            if (this.isEditing && this.currentTaskIndex === index) {
                this.isEditing = false
                this.newTask = ''
                this.currentTaskIndex = null
            }
        }
    }
}
</script>

<style scoped>
.todo-list {
    max-width: 400px;
    margin: 0 auto;
    text-align: center;
    color: #fff;
}

input {
    width: 80%;
    padding: 10px;
    margin-bottom: 10px;
}

button {
    margin-left: 5px;
    padding: 5px 10px;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    margin: 5px 0;
    padding: 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    border: 1px solid #ddd;
    color: #fff;
}

.actions {
    display: flex;
    gap: 10px;
}

.completed {
    text-decoration: line-through;
    color: #888;
}

button {
    background-color: #007bff;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease, transform 0.2s ease;
}

button:hover {
    background-color: #0056b3;
}

button:active {
    transform: scale(0.98);
}

button.edit {
    background-color: #28a745;
}

button.edit:hover {
    background-color: #218838;
}

button.delete {
    background-color: #dc3545;
}

button.delete:hover {
    background-color: #c82333;
}
</style>