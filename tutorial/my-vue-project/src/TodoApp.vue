<script setup>
import { ref, computed, watch, onMounted } from 'vue'
const newTask = ref('')
const tasks = ref([])

const editingId = ref(null)
const editingBuffer = ref("")

const search = ref('');
const filters = ['All', 'Completed', 'Incompleted', 'Favorites'];
const activeFilter = ref('All');

onMounted(() => {
    const saved = localStorage.getItem('tasks');
    if(saved){
        tasks.value = JSON.parse(saved);
    }
})

function addTask() {
    const text = newTask.value.trim();
    if (!text) {
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

function removeTask(idToRemove) {
    tasks.value = tasks.value.filter((t => t.id !== idToRemove))
}

function startEdit(task) {
    editingId.value = task.id;
    editingBuffer.value = task.text;
}

function cancelEdit() {
    editingId.value = null;
    editingBuffer.value = "";
}

function finishEdit(task) {
    if (editingId.value !== task.id) {
        return;
    }

    const trimmed = editingBuffer.value.trim();

    if (!trimmed) {
        removeTask(task.id);
    } else {
        task.text = trimmed;
    }

    cancelEdit();
}

function toggleFav(task) {
    task.favorite = !task.favorite;
}

const filteredTasks = computed(() => {
    return tasks.value
        .filter(tsk => tsk.text.toLowerCase().includes(search.value.toLowerCase()))
        .filter(tsk => {
            if(activeFilter.value === 'Completed') return tsk.completed;
            if(activeFilter.value === 'Incompleted') return !tsk.completed;
            if(activeFilter.value === 'Favorites') return tsk.favorite;
            return true;
    });
});

watch(
    tasks, 
    () => {
        localStorage.setItem('tasks', JSON.stringify(tasks.value));
    },
    { deep: true},
)


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

                <div class="mb-3">
                    <label for="searchInput" class="form-label">Search</label>
                    <input type="text" class="form-control" id="searchInput" aria-describedby="searchHelp"
                        v-model="search">
                    <div id="searchHelp" class="form-text">Something</div>
                </div>

                <div class="btn-group mb-4" role="group" aria-label="Filter buttons">
                    <button @click="activeFilter = fil" v-for="fil in filters" :key="fil" class="btn btn-outline-dark"
                        :class="{ 'active': activeFilter === fil }">{{ fil }}</button>
                </div>

                <ul class="list-group">
                    <li v-for="task in tasks" class="list-group-item d-flex align-items-center justify-content-between"
                        :key="task.id">
                        <template v-if="editingId === task.id">
                            <input type="text" class="form-control" v-model="editingBuffer"
                                @keyup.enter="finishEdit(task)" :ref="el => el && el.focus()" />
                            <span class="float-end text-nowrap">
                                <button type="button" class="btn btn-success ms-2"
                                    @click="finishEdit(task)">Done</button>
                                <button type="button" class="btn btn-danger ms-2" @click="cancelEdit()">Cancel</button>
                            </span>
                        </template>
                        <template v-else>
                            <span>
                                <input class="form-check-input me-1" type="checkbox" v-model="task.completed"
                                    :id="task.id">
                                <label class="form-check-label ms-2"
                                    :class="{ 'text-decoration-line-through': task.completed }"
                                    :for="task.id">{{ task.text }}</label>
                                <span v-if="task.favorite" class="badge text-bg-warning ms-1">Favorite</span>
                            </span>
                            <span class="float-end text-nowrap">
                                <button type="button" class="btn btn-danger" @click="removeTask(task.id)">X</button>
                                <button type="button" class="btn btn-info ms-2" @click="startEdit(task)">Edit</button>
                                <button type="button" class="btn btn-warning ms-2"
                                    :class="[task.favorite ? 'btn-light' : 'btn-warning']" @click="toggleFav(task)">{{
                                    task.favorite ? 'Unfav' : 'Fav' }}</button>
                            </span>
                        </template>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>