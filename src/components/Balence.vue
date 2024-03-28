<script setup>
const props = defineProps(["balence"])
</script>

<template>
    <h4>Your Balance</h4>
    <h1 id="balance">${{ balence }}</h1>

</template>