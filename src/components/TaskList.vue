<template>
    <div class="task-list-container">
        <h2 v-once>Lista de Tarefas</h2>
        <div class="controls">

            <button 
                :class="changeBtnAddClass"
                @click="handleShowForm"
            >
                {{ btnAddText }}
            </button>

            <button class="btn-toggle-all">
                Marcar Todas
            </button>

            <button class="btn-clear">
                Limpar Concluídas
            </button>

        </div>

        <div
            v-if="showForm" 
            class="add-task-container"
        >
            <input 
                v-model="newTaskTitle"
                type="text" 
                placeholder="Digite o título da tarefa" 
                class="task-input"
            />
            <button 
                class="btn-add"
                @click="addNewTask"
            >
            Adicionar
        </button>

    </div>
    <div class="tasks-container">
        <div class="pending-tasks">
            <h3 v-once>Tarefas Pendentes</h3>
            <p v-if="pendingTasks.length === 0">Nenhuma tarefa pendente no momento...</p>
            <div v-else>
                <TaskItem
                    v-for="task in pendingTasks" 
                    v-memo="[task.done,task.title]"
                    :key="task.id"
                    :task="task"
                    @toggle-done="toggleTaskDone"
                    @remove-task="removeTask"
                    
                />
            </div>
        </div>
        <div class="completed-tasks">
            <h3 v-once>Tarefas Concluídas</h3>
            <p v-if="completedTasks.length === 0">Nenhuma tarefa concluida no momento...</p>
            <div v-else>
                <TaskItem
                    v-for="task in completedTasks"
                    v-memo="[task.done,task.title]"
                    :key="task.id"
                    :task="task"
                    @toggle-done="toggleTaskDone"
                    @remove-task="removeTask"
                
                />
            </div>
        </div>
    </div>

    <div>
        <h3 v-once>Resumo</h3>
        <p v-if="tasks.length === 0">
            Você ainda não possui tarefas.
        </p>
        <p v-else-if="pendingTasks.length > 0 && completedTasks.length === 0">
            Você tem {{ pendingTasks.length }} tarefa(s) pendente(s).
        </p>
        <p v-else-if="pendingTasks.length === 0 && completedTasks.length > 0">
            Todas as tarefas foram concluídas!
        </p>
        <p v-else>
            Você tem {{ pendingTasks.length }} pendente(s) e {{ completedTasks.length }} concluida(s).
        </p>
    </div>

    <div class="watch-output">
        <h3 v-once>Saída do Watch {Console}</h3>
        <div class="log-container">
            {{ watchLogs }}
        </div>
    </div>
</div>

</template>

<script>
import TaskItem from './TaskItem.vue';

export default {
    name: 'TaskList',
    components: {
        TaskItem
    },
    data() {
        return {
            tasks: [
                {id: 1, title: 'Tarefa exemplo 1', done: false},
                {id: 2, title: 'Tarefa exemplo 2', done: true}
            ],
            watchLogs: [],
            newTaskTitle: '',
            showForm: false
        }
    },
    beforeCreate() {
        console.log('beforeCreate chamado!');
        console.log(this.tasks);
    },
    created() {
        console.log('created chamado!');
        console.log(this.tasks);
    },
    beforeMount() {
        console.log('beforeMount chamado!');
    },
    mounted() {
        console.log('mounted chamado!');
    },
    beforeUpdate() {
        console.log('beforeUpdate antes de atualizar as árvores dos componentes!');
    },
    updated() {
        console.log('updated depois de atualizar!');
    },
    methods: {
        removeTask(taskId) {
            this.tasks = this.tasks.filter(task => task.id !== taskId);
        },
        toggleTaskDone(taskId) {
            const task = this.tasks.find(task => task.id === taskId);
            if(task) {
                task.done = !task.done;
            }
        },
        logWatch(message) {
            this.watchLogs.unshift(`${new Date().toLocaleString()} - ${message}`);
        },
        addNewTask() {
            if(this.newTaskTitle.trim() === '') return;
            
            this.tasks.push({
                id: Date.now(),
                title: this.newTaskTitle.trim(),
                done: false
            });

            this.newTaskTitle = '';
            this.showForm = false;
        },
        handleShowForm() {
            this.showForm = !this.showForm;
        }
    },
    watch: {
        tasks: {
            handler(newValue, oldValue) {
                const message = `Lista de Tasks mudou! Itens: ${newValue.length}`;
                this.logWatch(message);
                if(oldValue) {
                    const modified = newValue.filter(n =>{
                        const oldTask = oldValue.find(o => o.id === n.id);
                        return oldTask && JSON.stringify(n) !== JSON.stringify(oldTask);
                    });
                    if(modified.length > 0) {
                        const modifyMsg = `Tarefas modificadas: ${modified.map(t => t.id).join(', ')}`;
                        this.logWatch(modifyMsg);
                    }
                }
            },
            deep: true,
            immediate: true
        }
    },
    computed: {
        completedTasks() {
            return this.tasks.filter(task => task.done);
        },
        pendingTasks() {
            return this.tasks.filter(task => !task.done);
        },
        btnAddText() {
            return this.showForm ? 'Fechar' : 'Adicionar Nova Tarefa'
        },
        changeBtnAddClass() {
            return this.showForm ? 'btn-clear' : 'btn-add'
        }
    }
}
</script>
<style>
.task-list-container {
    max-width: 1000px;
    margin: 0 auto;
    padding: 20px;
    
}
h2 {
    color: #0f74b8;
    margin-bottom: 20px;
    padding-bottom: 10px;
    border-bottom: 2px solid #ddd;
    text-align: center;
}
.controls {
    display: flex;
    gap: 10px;
    margin-bottom: 30px;
    flex-wrap: wrap;
    justify-content: center;
}
button {
    padding: 10px 20px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-weight: bold;
    transition: all 0.3s;
    font-size: 14px;
}
.btn-add{
    background-color: #09b863;
    color: white;
}
.btn-add:hover{
    background-color: #66e69b;
    transform: translateY(-2px);
}
.btn-toggle-all{
    background-color: #0c81a9;
    color: white;
}
.btn-toggle-all:hover{
    background-color: #66d9e6;
    transform: translateY(-2px);
}
.btn-clear{
    background-color: #e73838;
    color: white;
}
.btn-clear:hover{
    background-color: #f6a3a3;
    transform: translateY(-2px);
}
.task-container {
    display: grid;
    grid-template-columns: 2;
    gap: 30px;
    margin-bottom: 30px;
}
.pending-tasks, .completed-tasks {
    padding: 20px;
    border-radius: 8px;
}
.pending-tasks {
    background-color: #f9f9f9;
    border: 2px solid #eee;
}
.completed-tasks {
    background-color: #f0fff0;
    border: 2px solid #d4edda;
}
.pending-tasks h3, .completed-tasks h3 {
    margin-top: 0px;
    margin-bottom: 15px;
    color: #021d30;
}
.watch-output {
    background-color: #2c3e50;
    padding: 15px;
    border-radius: 6px;
    color: white;
}
.log-container {
    max-height: 200px;
    overflow-y: auto;
    background-color: #1a252f;
    padding: 10px;
    border-radius: 4px;
    margin-top: 10px;
    font-family: monospace;
}
.add-task-container {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
    justify-content: center;
}
.task-input {
    padding: 10px;
    border-radius: 6px;
    border: 1px solid #ccc;
    width: 300px;
}
</style>