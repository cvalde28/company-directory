<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { useRoute } from 'vue-router';
import useAPI from '@/composables/useAPI';
import { faker } from '@faker-js/faker';

const route = useRoute(); // Declare route first
const { fetchEmployee, currentEmployee } = useAPI(); // Call useAPI as a function

onMounted(async () => {
    if (route.params.id) {
        await fetchEmployee(route.params.id); // Pass the ID from route parameters
    }
});

onUnmounted(() => {
    currentEmployee.value = null; // Cleanup when the component is unmounted
});
</script>

<template>
    <main>
        <div v-if="currentEmployee" class="flex flex-col items-center justify-center gap-6">
            <h1 class="text-6xl font-bold p-5">
                {{ currentEmployee.firstName }} {{ currentEmployee.lastName }}
            </h1>
            <h1 class="text-3xl p-5">{{ currentEmployee.title }}</h1>
            <h1 class="text-2xl p-5">{{ currentEmployee.userName }}@southtexascollege.edu</h1>
            <h1 class="text-2xl p-5">{{ currentEmployee.quote }}</h1>

            <img class="p-8" :src="faker.image.urlLoremFlickr({ category: 'cat' })" />
        </div>
        <div v-else class="text-center text-2xl p-10">
            Loading employee details...
        </div>
    </main>
</template>
