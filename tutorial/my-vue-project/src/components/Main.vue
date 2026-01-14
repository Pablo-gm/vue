<script setup>
    import {ref} from 'vue'
    const quote = ref('So here we are I guess.')
    const author = ref('John Quoter')
</script>

<template>
    <div class="card shadow-sm mb-4 bg-success-subtle" >
        <div class="card-body">
            <p class="fs-3">"{{ quote }}"</p>
            <div class="text-end"><span class="text-body-secondary">{{ author }}</span></div>
            <button type="button" class="w-100 btn btn-lg btn-success mt-3">New quote</button>
        </div>
    </div>
</template>