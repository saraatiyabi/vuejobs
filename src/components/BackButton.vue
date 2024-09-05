<script lang="ts" setup>
import { RouterLink } from 'vue-router';
</script>

<template>
    <section>
        <div class="container m-auto pt-6 px-6">
            <RouterLink to="/jobs" class="text-green-500 hover:text-green-600 flex items-center bg-white py-3 px-5 rounded-3xl font-bold mt-5">
                <i class="pi pi-arrow-circle-left text-xl mr-3"></i> Back to Job Listings
            </RouterLink>
        </div>
    </section>
</template>
