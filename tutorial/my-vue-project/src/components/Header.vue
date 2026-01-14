<script setup>
    import {ref} from 'vue';
    const headerText = ref('Quote generator')
</script>

<template>
    <header>
        <h1 class="mb-4">{{headerText}}</h1>
    </header>
</template>

<style scoped>
    header span{
        font-size: 3rem;
    }
</style>