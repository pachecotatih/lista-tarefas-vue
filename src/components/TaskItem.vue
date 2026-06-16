<template>
    <div class="task-item">
        <span class="task-id"># {{task?.id}}</span>
        <span class="task-id">{{task?.title}}</span>
        <button 
            class="btn-toggle" 
            @click="toggleDone"
        >
            {{btnToggleText}}
        </button>
        <button 
            class="btn-remove" 
            @click="removeTask"
        >
            Remover
        </button>
    </div>
</template>

<script>
export default {
    name: 'TaskItem',
    props: {
        task: {
            type: Object,
            required: true
        }
    },
    methods: {
        toggleDone() {
            this.$emit('toggle-done', this.task.id)
        },
        removeTask() {
            this.$emit('remove-task', this.task.id)
        }
    },
    computed: {
        btnToggleText() {
            return this.task?.done ? 'Desfazer' : 'Concluir'
        },
    }
}
</script>
<style>
.task-item {
    display: flex;
    align-items: center;
    padding: 10px;
    margin-bottom: 8px;
    border-radius: 4px;
    background-color: white;
    border: 1px solid #ddd;
    transition: all 0.3s;
}

.task-id {
    font-weight: bold;
    margin-right: 10px;
    color: #4d0d75;

}


.btn-toggle {
    background-color: #704c90;
    color: white;
    margin-right: 5px;
}

.btn-toggle:hover {
    background-color: #4d0d75;
}

.btn-remove {
    background-color: #e64747;
    color: white;
}

.btn-remove:hover {
    background-color: #b31515;
}
</style>