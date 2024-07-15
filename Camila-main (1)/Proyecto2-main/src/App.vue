<script setup>
import { RouterLink,RouterView, useRouter } from 'vue-router';
import { onMounted } from 'vue';
import 'bootstrap-icons/font/bootstrap-icons.css';
const router = useRouter();
onMounted(() => {
    router.push('/');
});
</script>
<template>
    <div class="app" style="height: 100vh">
        <RouterView />
    </div>
</template>

<style scoped>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Arial, Helvetica, sans-serif;
}
</style>
