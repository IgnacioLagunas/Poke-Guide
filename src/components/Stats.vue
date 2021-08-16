<template>
    <div>
        <h2>{{title}}</h2>

        <p v-for="stat in stats" :key="stat">{{stat}}</p>
    </div>
</template>
<script>
    export default {
        
        name: "Stats",

        props:{
            title: String,
            stats: Array
        }
    }
</script>
<style>

</style>