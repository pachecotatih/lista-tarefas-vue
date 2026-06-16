<template>
   <button @click="$emit('update')">{{title}}</button>
</template>

<script>
export default {
    name: 'MyButton',
    props: {
        title: {
            type: String,
            default: "+1"
        }
    }
}
</script>
<style>
button {
    padding: 10px 20px;
    background-color: #42b809;
    color: white;
    border: none;
    border-radius: 8px;
    cursor: pointer;
}

button:hover{
    background-color: #b1e666;
}
</style>