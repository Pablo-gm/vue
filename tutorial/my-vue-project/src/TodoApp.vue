<script setup>
import { ref } from 'vue'
const newTask = ref('')
const tasks = ref([])

const editingId = ref(null)
const editingBuffer = ref("")

function addTask(){
    const text = newTask.value.trim();
    if(!text){
        return
    }

    tasks.value.push({
        id: Date.now(),
        text: text,
        completed: false,
        favorite: false
    });

    newTask.value = '';
    console.log(tasks);
}

function removeTask(idToRemove){
    tasks.value = tasks.value.filter(( t => t.id !== idToRemove))
}

function startEdit(task){
    editingId.value = task.id;
    editingBuffer.value = task.text
}
</script>

<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-lg-4">
                <h1 class="mt-5 mb-3">Todo app</h1>
                <div class="card mb-4">
                    <div class="card-body">
                        <div class="mb-3">
                            <label for="newTask" class="form-label">New task</label>
                            <input type="text" class="form-control" id="newTask" aria-describedby="newTask"
                                v-model="newTask">
                            <div id="newTask" class="form-text">Add task here</div>
                        </div>
                        <button type="button" class="btn btn-primary" @click="addTask">Submit</button>
                    </div>
                </div>

                <ul class="list-group">
                    <li v-for="task in tasks" class="list-group-item d-flex align-items-center justify-content-between" :key="task.id">
                        <span>
                            <input class="form-check-input me-1" type="checkbox" v-model="task.completed" :id="task.id">
                            <label class="form-check-label ms-2" :class="{'text-decoration-line-through': task.completed}"  :for="task.id">{{task.text}}</label>
                        </span>
                        <span class="float-end">
                            <button type="button" class="btn btn-danger" @click="removeTask(task.id)">X</button>
                            <button type="button" class="btn btn-info" @click="editTask(task.id)">Edit</button>
                            <button type="button" class="btn btn-warning ms-2">Fav</button>
                        </span>
                        
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>